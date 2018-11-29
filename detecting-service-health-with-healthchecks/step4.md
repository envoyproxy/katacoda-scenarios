Next we want to test how Envoy handles a node becoming unheathy. 

In a separate terminal window, launch a loop that will send requests. This will allow you to identify the changes in status.

`while true; do curl localhost; sleep .5; done`{{execute T2}}

## Mark Node Unheathy

With the following command, you can identify which Docker Container has the IP `172.18.0.3`. This will be the node that will become unheathy and later be removed from the load balancing rotation.

`docker ps -q | xargs -n 1 docker inspect --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}} {{ .ID }}' | sed 's/ \// /'`{{execute T1}}

To make the node unheathy, call the endpoint `curl 172.18.0.3/unhealthy`{{execute T1}}

This will cause all future requests to return a 500 error message `curl 172.18.0.3 -i`{{execute T1}}

During this time, Envoy is sending requests to the health endpoint. If the health endpoint fails, it will continue to send traffic to the service until the `unhealthy_threshold` has been reached. At this point it will remove it from the load balancer rotation.