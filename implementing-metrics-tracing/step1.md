`cat prometheus.yml`{{execute}}

```
docker run -d -p 9090:9090 \
    -v /root/prometheus.yml:/etc/prometheus/prometheus.yml \
    --name prometheus-server \
    prom/prometheus
```{{execute}}

https://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com/targets