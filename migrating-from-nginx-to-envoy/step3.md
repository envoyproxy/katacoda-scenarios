Within the HTTP configuration block, the NGINX configuration specifies to listen on port 8080 and respond to incoming requests for the domains _one.example.com_ and _www.one.example.com_.

<pre>
 server {
    listen        80;
    server_name   one.example.com  www.one.example.com;
</pre>


## Envoy Listeners

The most important aspect to start with Envoy Proxy is defining the listers.

<pre data-filename="envoy.yaml">
static_resources:
  listeners:
  - name: listener_0
    address:
      socket_address: { address: 0.0.0.0, port_value: 8080 }
</pre>

There is no need to define the *server_name* as this will be handled via the Envoy Proxy Filters.