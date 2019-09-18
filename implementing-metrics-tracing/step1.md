Prometheus is an open-source monitoring solution that can be integrated with Envoy's own stats.

Now, let's see the prometheus configuration:

`prometheus.yml`{{open}}

We can see that the prometheus config is using a job named  `envoy`.

```
scrape_configs:
  - job_name: 'envoy'
    metrics_path: /stats/prometheus
```

Start prometheus with command:

```
docker run -d -p 9090:9090 \
    -v /root/envoy/prometheus.yml:/etc/prometheus/prometheus.yml \
    --name prometheus-server \
    prom/prometheus
```{{execute}}

You can access to the prometheus dashboard using this link:

https://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com/targets
