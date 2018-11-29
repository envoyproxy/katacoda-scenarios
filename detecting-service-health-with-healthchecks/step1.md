An initial envoy configuration file has been created at `envoy.yaml`{{open}}

This is configured to proxy all incoming requests for any domain to `172.18.0.3` and `172.18.0.4`.

However, at the moment, if `172.18.0.3` went down then Envoy would still continue to deliver traffic to the service. As a result, users would have a degraded experience and encounter errors.

Instead, we want Envoy to automatically detect that the service is unavailable and remove it from the load balancer rotation. This is done by adding health checks to the cluster which will be defined in the next step.