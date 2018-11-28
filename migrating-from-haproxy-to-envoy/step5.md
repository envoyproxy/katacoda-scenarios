The Envoy Proxy configuration has now been translated from HA Proxy. The final section is to launch the Envoy Proxy instance to test it.

## Run As User

At the top of the configuration, the line `user haproxy` indicates to run HA Proxy as a low privileged user.

Envoy Proxy takes a cloud native approach to managing who the process owner is. When we launch Envoy Proxy via a Container we can specify a low privileged user.

## Starting Envoy Proxy

Below will launch Envoy Proxy via a Docker Container on the host. The command exposes Envoy to listen to incoming requests on port 80. However, as specified in the listener configuration, Envoy Proxy is listening to incoming traffic on port 8080. This allows the process to run as a low privilege user.

`docker run --name proxy1 -p 80:8080 --user 1000:1000 -v /root/envoy.yaml:/etc/envoy/envoy.yaml envoyproxy/envoy`{{execute T2}}

## Testing

With the Proxy started, tests can now be made and processed. The following cURL command issues a request with the Host Header defined in the proxy configuration.

`curl -H "Host: one.example.com" localhost -i`{{execute T1}}

The HTTP request will result in a _503_ error because the upstream connections are not running and unavailable. As such, Envoy Proxy has no available target destinations for the request. The following command will launch a series of HTTP services that match the configuration defined for Envoy. 

`docker run -d katacoda/docker-http-server; docker run -d katacoda/docker-http-server;`{{execute T1}}

With the services available, Envoy can successfully proxy traffic to the target destination.

`curl -H "Host: one.example.com" localhost -i`{{execute T1}}

You should see a response indicating which Docker Container processed the request. Within the Envoy Proxy logs you should also see the access line outputted.

## Additional HTTP Response Headers

Within the response headers of the valid request you will see additional HTTP Headers. The Header contains the time in milliseconds spent by the upstream host processing the request. This is useful if the client wants to determine service time compared to network latency. 

```
x-envoy-upstream-service-time: 0
server: envoy
```
