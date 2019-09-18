Now, let's run grafana with this command:

`docker run --name=grafana -d -p 3000:3000 grafana/grafana`{{execute T1}}

And access to the dashboard using this url:

https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/

And use this default credentials:
`admin`{{copy}} \ `admin`{{copy}}

The initial step will ask you to change the password, you can do it if you want or you can skip this step.

The first step to create a dashboard is to have a datasource. Let's define a datasource with phometheus data configured before:

The URL for phometheus should be:

`http://172.18.0.5:9090`{{copy}}

For all the other fields you can use the default values

![](/envoyproxy/scenarios/implementing-metrics-tracing/assets/prometheus-data-source.png)

Test and save your datasource.

## Build a dashboard

In order to build a dashboard you can build one from scratch or you can use an existing one, for example:
https://grafana.com/dashboards/6693

Let's use this existing dashboard. Copy the ID, and use the option `Import`.

`6693`{{copy}}

Select **Prometheus** as the data source and Import.

![](/envoyproxy/scenarios/implementing-metrics-tracing/assets/import.png)

[View Dashboard for the targetCluster](https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/d/000000003/envoy-proxy?refresh=5s&orgId=1&var-cluster=targetCluster&var-hosts=All)

## Change Traffic

`while true; do curl localhost; sleep .1; done`{{execute interrupt T2}}

Should see a big spike in the traffic on the dashboard.

## Generate Errors

`curl 172.18.0.3/unhealthy; curl 172.18.0.4/unhealthy;`{{execute}}

Within Prometheus, you will also see a increase in the number of 500 error messages.

https://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com/graph

`envoy_cluster_external_upstream_rq{envoy_cluster_name="targetCluster"}`{{copy}}
