<pre class="file"  data-filename="envoy.yaml" data-target="replace">admin:
  access_log_path: /tmp/admin_access.log
  address:
    socket_address: { address: 0.0.0.0, port_value: 9901 }
</pre>

<pre class="file"  data-filename="envoy.yaml" data-target="append">static_resources:</pre>

<pre class="file"  data-filename="envoy.yaml" data-target="append">  listeners:
  - name: listener_0
    address:
      socket_address: { address: 0.0.0.0, port_value: 10000 }
</pre>

<pre class="file"  data-filename="envoy.yaml" data-target="append">    filter_chains:
    - filters:
      - name: envoy.http_connection_manager
        config:
          stat_prefix: ingress_http
          route_config:
            name: local_route
            virtual_hosts:
            - name: local_service
              domains: ["*"]
              routes:
              - match: { prefix: "/" }
                route: { host_rewrite: www.google.com, cluster: service_google }
          http_filters:
          - name: envoy.router
</pre>

<pre class="file"  data-filename="envoy.yaml" data-target="append">  clusters:
  - name: service_google
    connect_timeout: 0.25s
    type: LOGICAL_DNS
    dns_lookup_family: V4_ONLY
    lb_policy: ROUND_ROBIN
    hosts: [{ socket_address: { address: google.com, port_value: 443 }}]
    tls_context: { sni: www.google.com }
</pre>

[View full configuration on Github](https://github.com/envoyproxy/envoy/blob/6a578630a8f6189f86bc1e6b4b4d7ebffabadadd/configs/google_com_proxy.v2.yaml)