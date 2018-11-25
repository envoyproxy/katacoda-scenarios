Within the HTTP configuration block, the HA Proxy configuration listenes on port 8080 and all traffic is handled by the backend nodes. 

<pre class="file">
frontend localnodes
    bind *:8080
    mode http
    default_backend nodes
</pre>

## Envoy Listeners

Within Envoy, the binding of configuration is defined as Listeners. Each listerner can define a port and a series of filters, routes and clusters that respond on that port. In this case, there is one listener defined bound to port 8080.

<pre class="file" data-filename="envoy.yaml" data-target="replace">
static_resources:
  listeners:
  - name: listener_0
    address:
      socket_address: { address: 0.0.0.0, port_value: 8080 }
</pre>

In the next step, we'll find our routes and cluster that will handle the traffic.