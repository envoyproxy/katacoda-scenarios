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