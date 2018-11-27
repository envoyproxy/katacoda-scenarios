Envoy has detected that the service is unavailable and returning error messages. Envoy will continue to check the service to see if it becomes available again.

## Task

Make the unavailable server healthy again with the command `curl 172.18.0.2/healthy`{{execute T1}}

After a few moments, 10s based on the envoy internal, Envoy should detect that the service is healthy again.

The curl commands should indicate two servers available.

`curl localhost -i`{{execute}}

It's possible to identify the health status of the cluster via....