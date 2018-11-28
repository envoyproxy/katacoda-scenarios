When a request comes into NGINX a location block defines how to process and where to forward the traffic. In the following snippet, all the traffic to the site is proxied to an upstream cluster called `targetCluster`. The upstream cluster defines the nodes that should process the request, which will be discussed in the next step.

<pre class="file">
location / {
    proxy_pass         http://targetCluster/;
    proxy_redirect     off;

    proxy_set_header   Host             $host;
    proxy_set_header   X-Real-IP        $remote_addr;
}
</pre>

Within Envoy, this is managed by Filters.

## Envoy Filterers

For the static configuration, the filters define how to handle incoming requests. In this case, we are setting the filters that match the *server_names* in the previous step. When incoming requests are received that match the defined domains and routes, the traffic will be forwarded to the cluster. This is the equivalent of the upstream configuration. 

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
                - "one.example.com"
                - "www.one.example.com"
              routes:
              - match:
                  prefix: "/"
                route:
                  cluster: targetCluster
          http_filters:
          - name: envoy.router
</pre>

The name **envoy.http_connection_manager** is a built-in filter within Envoy Proxy. Other filters include Redis, Mongo, TCP with the complete list found in the [documentation](https://www.envoyproxy.io/docs/envoy/latest/api-v2/api/v2/listener/listener.proto#envoy-api-file-envoy-api-v2-listener-listener-proto)