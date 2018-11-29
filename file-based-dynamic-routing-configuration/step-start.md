With the Envoy configuration and EDS conf defined, it's now possible to start the proxy. 

## Task

Launch the container with the following command:

```
docker run --name=proxy-eds-filebased -d \
    -p 9901:9901 \
    -p 80:10000 \
    -v /root/:/etc/envoy \
    envoyproxy/envoy:latest
```{{execute}}

Start two HTTP servers to handle the incoming requests.

`docker run -d katacoda/docker-http-server; docker run -d katacoda/docker-http-server;`{{execute}}

Based on the current EDS configuration, Envoy will send all the traffic to a single node. Try it with `curl localhost`{{execute}}

In the next step we'll update the configuration and add an additional node.