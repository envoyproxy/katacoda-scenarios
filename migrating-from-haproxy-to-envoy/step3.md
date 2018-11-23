The backend configuration defines the load balancer configuration to handle the incoming traffic. In this configuration example, three nodes have been defined in a round-robin fashion.

<pre>
backend nodes
    mode http
    balance roundrobin
    option forwardfor
    server web01 172.17.0.2:9000 check
    server web02 172.17.0.3:9000 check
    server web03 172.17.0.4:9000 check

</pre>

Within Envoy, this functionality is handled by creating filters and clusters.

## Envoy Filterers and Clusters

For the static configuration, the filters define how to handle incoming requests. In this case we are defining the filters that matches the *server_names* in the previous step. When requests are made that match the defined domains and routes, the traffic will be forwarded to the cluster. This is the equviaint of the upstream configuration. 

<pre data-filename="envoy.yaml">
  - filters:
    - name: envoy.http_connection_manager
    config:
        codec_type: auto
        stat_prefix: ingress_http
        route_config:
        name: local_route
        virtual_hosts:
        - name: backend
            domains:
            - "*"
            routes:
            - match:
                prefix: "/"
            route:
                cluster: nodes
        http_filters:
        - name: envoy.router
        config: {}
</pre>

The filter controls how Envoy matches incoming HTTP requests and which cluster should handle them. The cluster contrils which servers are handling the traffic and the load balancing configuration, such as Round Robin.

<pre>
  clusters:
  - name: nodes
    connect_timeout: 0.25s
    type: LOGICAL_DNS
    dns_lookup_family: V4_ONLY
    lb_policy: ROUND_ROBIN
    hosts: [
      { socket_address: { address: 172.17.0.2, port_value: 9000 }},
      { socket_address: { address: 172.17.0.3, port_value: 9000 }},
      { socket_address: { address: 172.17.0.4, port_value: 9000 }},
    ]
</pre>

