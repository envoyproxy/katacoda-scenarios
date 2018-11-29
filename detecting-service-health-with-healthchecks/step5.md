In the background, Envoy will continue to check the health endpoint to see if it becomes available again. Once it has, it will be returned to the load balancer rotation.

## Task

Make the unavailable server healthy again with the command `curl 172.18.0.3/healthy`{{execute T1}}

After a few moments, 10s based on the envoy `internal` value, Envoy should detect that the service is healthy again.

When Envoy Proxy now handles the incoming requests, they should be load balanced across the two services again.

`curl localhost -i`{{execute}}
