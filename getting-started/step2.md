`docker run --name=proxy -d \
    -p 9901:9901 \
    -p 10000:10000 \
    -v $(pwd)/envoy/envoy.yaml:/etc/envoy/envoy.yaml \
    envoyproxy/envoy:latest`{{execute}}

https://[[HOST_SUBDOMAIN]]-10000-[[KATACODA_HOST]].environments.katacoda.com/


https://[[HOST_SUBDOMAIN]]-9901-[[KATACODA_HOST]].environments.katacoda.com/
