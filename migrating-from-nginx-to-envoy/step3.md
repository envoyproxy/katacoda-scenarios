Within the HTTP configuration block, the NGINX configuration specifies to listen on port 8080 and respond to incoming requests for the domains _one.example.com_ and _www.one.example.com_.

<pre class="file">
 server {
    listen        80;
    server_name   one.example.com  www.one.example.com;
</pre>


## Envoy Listeners

The most important aspect to start with Envoy Proxy is defining the listers. This is where we need to start creating our configuration file that defines how we want to run our envoy instance.

The snippet below will create a new listener and bind it to port 8080. This indicates to Envoy Proxy which ports it should be bind to for incoming requests.

<pre class="file" data-filename="envoy.yaml">
static_resources:
  listeners:
  - name: listener_0
    address:
      socket_address: { address: 0.0.0.0, port_value: 8080 }
</pre>

There is no need to define the *server_name* as this will be handled via the Envoy Proxy Filters.