The Envoy Proxy configuration has now been translated from HA Proxy. The final part is to launch it.

## Run As User

At the top of the configuration, the line `user haproxy` indicates to run HA Proxy as a low priviledged user. 

Envoy Proxy takes a cloud native approach to managing who the process owner is. When we launch Envoy Proxy via a Container we can specify a low priviledged user.

## Starting Envoy Proxy

Below will launch Envoy Proxy via a Docker Container on the host. The command exposes Envoy to listen to incoming requests on port 80. However, as specified in the listener configuraton, Envoy Proxy is listening to incoming traffic on port 8080. This allows the process to run as a low priv user.

`docker run --name proxy1 -p 80:8080 --user 1000:1000 -v /root/envoy/envoy.yaml:/etc/envoy/envoy.yaml envoyproxy/envoy`{{execute T2}}

## Testing

`curl -H localhost -i`{{execute T1}}

This will result in a _503_ error because the upstream connections are not running or available. 

`docker run -d katacoda/docker-http-server; docker run -d katacoda/docker-http-server;`{{execute T1}}

`curl -H localhost -i`{{execute T1}}

Additional headers...

```
x-envoy-upstream-service-time: 0
server: envoy
```
