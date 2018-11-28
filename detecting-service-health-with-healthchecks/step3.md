`docker run -d katacoda/docker-http-server:healthy; docker run -d katacoda/docker-http-server:healthy;`{{execute T1}}

`docker run --name proxy1 -p 80:8080 --user 1000:1000 -v /root/:/etc/envoy envoyproxy/envoy`{{execute T2}}

## Testing

`curl localhost -i`{{execute T1}}
