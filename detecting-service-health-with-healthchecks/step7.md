Unlike active health checking, *Outlier Detection* (sometimes called passive health checking) uses the responses from real requests to determine whether an endpoint is healthy. Once an endpoint is removed, Envoy uses a time-out based approach for re-insertion, where unhealthy hosts are re-added to the cluster after a configurable interval. Each subsequent removal increases the interval, so a persistently unhealthy endpoint does as little damage as possible to user traffic.

Like health checks, outlier detection is configured per-cluster. This configuration removes a host for 30 seconds when it returns 3 consecutive 5xx errors:

Open the file `envoy1.yaml`{{open}} and add the following configuration:

<pre class="file" data-filename="envoy1.yaml" data-target="append">
    outlier_detection:
        consecutive_5xx: "3"
        base_ejection_time: "30s"
</pre>

The key fields are:

* **consecutive_5xx**: If an upstream host returns some number of consecutive 5xx, it will be ejected. Note that in this case a 5xx means an actual 5xx respond code, or an event that would cause the HTTP router to return one on the upstream’s behalf (reset, connection failure, etc.).

* **base_ejection_time**: The base time that a host is ejected for. The real time is equal to the base time multiplied by the number of times the host has been ejected. Defaults to 30000ms or 30s.

More details on the API can be found at [Envoy documentation](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/outlier?highlight=outlier%20detection).