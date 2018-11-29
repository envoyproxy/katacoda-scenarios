With the new health check in place, Envoy will check the health of each node defined within the cluster.

## Task

Start Envoy with the command: 

`docker run -d --name proxy1 -p 80:8080 -v /root/:/etc/envoy envoyproxy/envoy`{{execute T1}}

Start two nodes, both start in a heathy state.

`docker run -d katacoda/docker-http-server:healthy; docker run -d katacoda/docker-http-server:healthy;`{{execute T1}}

## Testing

When sending requests to Envoy, healthy requests should be returned from the HTTP servers.

`curl localhost -i`{{execute T1}}
