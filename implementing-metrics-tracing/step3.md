`docker run --name=proxy -d -p 80:8080 envoyproxy/envoy`{{execute}}

`docker run --name=jaeger -d -p 6831:6831 jaeger`{{execute}}

`docker run --name=prom -d -p 9001:9001 promethues`{{execute}}

`docker run --name=grafana -d -p 80:80 grafana`{{execute}}