## Configuration 

`cd ~/root/envoy/examples/front-proxy`{{execute}}

`/root/envoy/examples/front-proxy/docker-compose.yaml`{{open}}

`/root/envoy/examples/front-proxy/front-envoy.yaml`{{open}}

`/root/envoy/examples/front-proxy/service-envoy.yaml`{{open}}


## Deploy 

`docker-compose -f ~/envoy/examples/front-proxy/docker-compose.yaml up -d`{{execute}}

## View

https://[[HOST_SUBDOMAIN]]-8001-[[KATACODA_HOST]].environments.katacoda.com/


https://[[HOST_SUBDOMAIN]]-8000-[[KATACODA_HOST]].environments.katacoda.com/service/1

https://[[HOST_SUBDOMAIN]]-8000-[[KATACODA_HOST]].environments.katacoda.com/service/2