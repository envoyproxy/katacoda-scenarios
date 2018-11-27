The configuration has now been translated from NGINX to Envoy Proxy. The final section is to launch it.

## Run As User

At the top of the configuration, the line `user  www www;` indicates to run NGINX as a low privileged user. This limits the access NGINX has and ensures a secure system.

Envoy Proxy takes a cloud native approach to managing who the process owner is. When we launch Envoy Proxy via a Container we can specify a low privileged user.

## Starting Envoy Proxy

Below will launch Envoy Proxy via a Docker Container on the host. The command exposes Envoy to listen to incoming requests on port 80. However, as specified in the listener configuration, Envoy Proxy is listening to incoming traffic on port 8080. This allows the process to run as a low privileged user.

`docker run --name proxy1 -p 80:8080 --user 1000:1000 -v /root/envoy/envoy.yaml:/etc/envoy/envoy.yaml envoyproxy/envoy`{{execute T2}}

## Testing

`curl -H "Host: one.example.com" localhost -i`{{execute T1}}

This will result in a _503_ error because the upstream connections are not running or available.

`docker run -d katacoda/docker-http-server; docker run -d katacoda/docker-http-server;`{{execute T1}}

`curl -H "Host: one.example.com" localhost -i`{{execute T1}}

Additional headers...

```
x-envoy-upstream-service-time: 0
server: envoy
```

## Hot Reloading

Learn more at the docs...