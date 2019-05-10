The backend configuration defines the load balancer configuration to handle the incoming traffic. In this configuration example, two nodes have been defined in a round-robin fashion.

<pre class="file">
backend nodes
    mode http
    balance roundrobin
    option forwardfor
    server web01 172.18.0.3:80 check
    server web02 172.18.0.4:80 check
</pre>

Within Envoy, this functionality is handled by creating filters and clusters.

## Envoy Filters and Clusters

For the static configuration, the filters define how to handle incoming requests. In this case, we are defining the filters that match with all the traffic. When requests are made that match the defined domains and routes, the traffic is forwarded to the cluster. This is the equivalent of the upstream configuration:

<pre class="file" data-filename="envoy.yaml">
    filter_chains:
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
</pre>

The name *envoy.http_connection_manager* is a built-in filter within Envoy Proxy. Other filters include _Redis_, _Mongo_, _TCP_. You can find the complete list in the [documentation](https://www.envoyproxy.io/docs/envoy/latest/api-v2/api/v2/listener/listener.proto#envoy-api-file-envoy-api-v2-listener-listener-proto).

The filter controls how Envoy matches incoming HTTP requests and which cluster should handle them. The cluster controls which servers are handling the traffic and the load balancing configuration, such as Round Robin.

<pre class="file" data-filename="envoy.yaml">
  clusters:
  - name: nodes
    connect_timeout: 0.25s
    type: STRICT_DNS
    dns_lookup_family: V4_ONLY
    lb_policy: ROUND_ROBIN
    hosts: [
      { socket_address: { address: 172.18.0.3, port_value: 80 }},
      { socket_address: { address: 172.18.0.4, port_value: 80 }}
    ]
</pre>

For more information about other load balancing policies visit the [Envoy documentation](https://www.envoyproxy.io/docs/envoy/v1.8.0/intro/arch_overview/load_balancing).