`docker run --name=grafana -d -p 3000:3000 grafana/grafana`{{execute T1}}

https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/

`admin`{{copy}} \ `admin`{{copy}}

`http://172.18.0.5:9090`{{copy}}

![](/envoyproxy/scenarios/implementing-metrics-tracing/assets/prometheus-data-source.png)

https://2886795274-3000-ollie02.environments.katacoda.com/dashboard/import

https://grafana.com/dashboards/6693

`6693`{{copy}}

Select **Prometheus** as the data source and Import.

[View Dashboard for the targetCluster](https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/d/000000003/envoy-proxy?refresh=5s&orgId=1&var-cluster=targetCluster&var-hosts=All)

## Change Traffic

`while true; do curl localhost; sleep .1; done`{{execute interrupt T2}}

Should see a big spike in the traffic on the dashboard.

docker ps -a | grep "docker-http-server" | awk '{print $1}' | xargs docker rm -f

## Generate Errors

`curl 172.18.0.3/unhealthy; curl 172.18.0.4/unhealthy;`{{execute}}

Within Prometheus, you will also see a increase in the number of 500 error messages.

https://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com/graph

`envoy_cluster_external_upstream_rq{envoy_cluster_name="targetCluster"}`{{copy}}