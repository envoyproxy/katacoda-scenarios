You can see the statistics generated in plan text in this URL:

https://[[HOST_SUBDOMAIN]]-9901-[[KATACODA_HOST]].environments.katacoda.com/stats/prometheus

And you can use one particular field to build a graph, for example:
`envoy_cluster_external_upstream_rq`

![](/envoyproxy/scenarios/implementing-metrics-tracing/assets/envoy_cluster_external_upstream_rq.png)

To build the graph, go to the dashboard:
https://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com/graph

And use this query:
```envoy_cluster_external_upstream_rq{envoy_cluster_name="targetCluster"}```{{copy}}
