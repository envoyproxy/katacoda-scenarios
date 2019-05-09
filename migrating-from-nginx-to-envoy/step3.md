Within the HTTP configuration block, the NGINX configuration specifies to listen on port 8080 and respond to incoming requests for the domains _one.example.com_ and _www.one.example.com_.

<pre class="file">
 server {
    listen        80;
    server_name   one.example.com  www.one.example.com;
</pre>

Within Envoy, this is managed by Listeners.

## Envoy Listeners

The most important aspect of starting with Envoy Proxy is defining the listers. You need to create a configuration file that describes how you want to run the Envoy instance.

The snippet below will create a new listener and bind it to port 8080. The configuration indicates to Envoy Proxy which ports it should be bound to for incoming requests.

<pre class="file" data-filename="envoy.yaml" data-target="replace">
static_resources:
  listeners:
  - name: listener_0
    address:
      socket_address: { address: 0.0.0.0, port_value: 8080 }
</pre>

There is no need to define the *server_name* as Envoy Proxy filters will handle this.