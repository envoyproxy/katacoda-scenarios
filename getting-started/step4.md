## Configuration 

`examples/front-proxy/docker-compose.yml`{{open}}

`examples/front-proxy/front-envoy.yaml`{{open}}

`examples/front-proxy/service-envoy.yaml`{{open}}


## Deploy 

`docker-compose -f ~/envoy/examples/front-proxy/docker-compose.yml up -d`{{execute}}

## View

Admin: https://[[HOST_SUBDOMAIN]]-8001-[[KATACODA_HOST]].environments.katacoda.com/config_dump
https://[[HOST_SUBDOMAIN]]-8001-[[KATACODA_HOST]].environments.katacoda.com/clusters


https://[[HOST_SUBDOMAIN]]-8000-[[KATACODA_HOST]].environments.katacoda.com/service/1

https://[[HOST_SUBDOMAIN]]-8000-[[KATACODA_HOST]].environments.katacoda.com/service/2