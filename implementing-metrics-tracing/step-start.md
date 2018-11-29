```
docker run --name=proxy -d \
    -p 9901:9901 \
    -p 80:10000 \
    -v /root/:/etc/envoy \
    envoyproxy/envoy:latest;
docker run -d katacoda/docker-http-server; 
docker run -d katacoda/docker-http-server;
```{{execute}}
