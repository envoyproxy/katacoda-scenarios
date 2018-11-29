<pre class="file" data-filename="envoy.yaml" data-target="append">
tracing:
  http:
    name: envoy.zipkin
    config:
      collector_cluster: 172.18.0.6
      collector_endpoint: "/api/v1/spans"
      shared_span_context: false
</pre>

`docker run -d --name jaeger -p 5775:5775/udp -p 16686:16686 jaegertracing/all-in-one:latest`{{execute}}