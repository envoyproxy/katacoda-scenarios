```
docker run --name=proxy-eds-filebased -d \
    -p 9901:9901 \
    -p 10000:10000 \
    -v $(pwd)/envoy:/etc/envoy \
    envoyproxy/envoy:latest
```{{execute}}