A health check is added to the cluster configuration within the Envoy configuration. The following configuration will use a `/health` endpoint within each of the nodes defined to determine if it is healthy or not, based on the HTTP Status returned.

<pre class="file" data-filename="envoy.yaml" data-target="append">
    health_checks:
      - timeout: 1s
        interval: 10s
        interval_jitter: 1s
        unhealthy_threshold: 6
        healthy_threshold: 1
        http_health_check:
          path: "/health"
</pre>

The key fields are:

* **interval**: How frequently should be health check be performed

* **unhealthy_threshold**: The number of unhealthy health checks required before a host is marked unhealthy

* **healthy_threshold**: The number of healthy health checks required before a host is marked healthy

* **http_health_check.path**: Specifies the HTTP path that will be requested during health checking

More details on the API can be found at [with the documentation](https://www.envoyproxy.io/docs/envoy/latest/api-v2/api/v2/core/health_check.proto).