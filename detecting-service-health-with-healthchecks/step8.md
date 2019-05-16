With the  Outlier Detection in place, Envoy will remove a host based on the responses from real requests.

## Testing

Before start, mark the nodes as healthy again, running the following command:
`curl 172.18.0.3/healthy; curl 172.18.0.4/healthy;`

Start Envoy with the command:

```
docker run -d --name proxy2 -p 81:8080 \
    -v /root/:/etc/envoy \
    -v /root/envoy1.yaml:/etc/envoy/envoy.yaml \
    envoyproxy/envoy
```{{execute}}

In a separate terminal window, launch a loop that will send requests. This will allow you to identify the changes in status.

`while true; do curl localhost:81; sleep .5; done`{{execute T2}}

## Mark Node Unhealthy

To make the node unheathy, call the endpoint `curl 172.18.0.3/unhealthy`{{execute T1}}

This will cause all future requests to return a 500 error message `curl 172.18.0.3:81 -i`{{execute T1}}

During this time, after the 3rd request with code `5xx`, Envoy will eject the node. You can see in the second terminal this behavior.

## Mark Node Healthy

You can mark the node healthy again, and see how Envoy will send traffic to that node again.
`curl 172.18.0.3/healthy`.

