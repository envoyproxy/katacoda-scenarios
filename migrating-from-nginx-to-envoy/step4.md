
<pre>
location / {
    proxy_pass         http://targetCluster/;
    proxy_redirect     off;

    proxy_set_header   Host             $host;
    proxy_set_header   X-Real-IP        $remote_addr;
}
</pre>


## Envoy Filterers

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
            - "one.example.com"
            - "www.one.example.com"
            routes:
            - match:
                prefix: "/"
            route:
                cluster: targetCluster
        http_filters:
        - name: envoy.router
        config: {}
</pre>