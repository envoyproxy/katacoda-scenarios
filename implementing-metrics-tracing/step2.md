Distributed tracing allows developers to obtain visualizations of call flows in large service oriented architectures. It can be invaluable in understanding serialization, parallelism, and sources of latency. For this example we will use Jaeger, an open source, end-to-end distributed tracing.

We can see the tracing configuration in `envoy.yaml`{{open}}

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

One important configuration for our example is telling to the connection manager that generates the
 `x-request-id` header if it does not exist.

 <pre class="file">
 generate_request_id: true
 tracing:
   operation_name: egress
 </pre>

`docker run -d --name jaeger -e COLLECTOR_ZIPKIN_HTTP_PORT=9411 -p 9411:9411 -p 5775:5775/udp -p 16686:16686 jaegertracing/all-in-one:latest`{{execute}}
