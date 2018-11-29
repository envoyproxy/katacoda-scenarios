This step will show what happens when all services are unavailable. In an ideal world, this will never happen but when it does it's important to understand the response to aid debugging and responsiveness. More information on [Debugging Envoy Proxy]() can be found within the [Interactive Scenario]().

At the moment there are two healthy HTTP servers running which Envoy Proxy is load balancing between them.

## Task

To make all the HTTP servers unhealthy, run the command `curl 172.18.0.3/unhealthy; curl 172.18.0.4/unhealthy;`{{execute}}. As before, this will trigger an endpoint to cause the HTTP server to fail.

## Envoy Response

Envoy Proxy's configuration has defined two endpoints, but they are now both unavailable.

When new requests come into Envoy, the proxy will return a 503 message with an error relating to upstream servers being unavailable. 

`curl localhost -i`{{execute}}
