<pre class="file">
tracing:
  http:
    name: envoy.zipkin
    config:
      collector_cluster: 172.18.0.6
      collector_endpoint: "/api/v1/spans"
      shared_span_context: false
</pre>

Ensure that Jaeger is configured to accept Zipkin requests via the *COLLECTOR_ZIPKIN_HTTP_PORT* Environment Variable.

`docker run -d --name jaeger -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 -p 9411:9411 -p 5775:5775/udp -p 16686:16686 jaegertracing/all-in-one:latest`{{execute}}