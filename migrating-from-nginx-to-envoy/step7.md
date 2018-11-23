The configuration has now been translated from NGINX to Envoy Proxy. The final section is to launch it.

## Run As User

At the top of the configuration, the line `user  www www;` indicates to run NGINX as a low priviledged user. This limits the access NGINX has and ensures a secure system.

Envoy Proxy takes a cloud native approach to managing who the process owner is. When we launch Envoy Proxy via a Container we can specify a low priviledged user.

## Starting Envoy Proxy

Below will launch Envoy Proxy via a Docker Container on the host. The command exposes Envoy to listen to incoming requests on port 80. However, as specified in the listener configuraton, Envoy Proxy is listening to incoming traffic on port 8080. This allows the process to run as a low priv user.

`docker run -p 80:8080 --user www envoyproxy`{{execute}}

## Hot Reloading

Learn more at the docs...