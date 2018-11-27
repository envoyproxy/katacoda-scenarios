This step will show what happens when all services are unavailable. In an ideal world, this will never happen, but when it does it's important to understand the response to aid debugging and responsiveness. More imformation on [Debugging Envoy Proxy]() can be found at []().

At the moment there are two healthy HTTP servers running which Envoy Proxy is load balancing between. 

## Task

To make all the HTTP servers unhealthy, run the command `curl 172.18.0.2/unhealthy; curl 172.18.0.3/unhealthy;`{{execute}}. As before, this will trigger a endpoint to cause the HTTP server to fail.

## Envoy Response

Envoy Proxy's configuration has defined two endpoints, but they are now both unavailable.

When new requests come in to Envoy, the proxy will return a 502 message. 

`curl localhost -i`{{execute}}
