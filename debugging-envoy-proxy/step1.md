Using the initial envoy configuration file:
`envoy/envoy.yaml`{{open}}

Start envoy with the following command:
`docker run --name proxy1 -p 80:10000 -p 9090:9090 --user 1000:1000 -v $(pwd)/envoy/:/etc/envoy  envoyproxy/envoy`{{execute interupt T2}}

And then start two healthy http servers using this command:
```
docker run -d katacoda/docker-http-server:healthy;
docker run -d katacoda/docker-http-server:healthy;
```{{execute interupt T3}}
