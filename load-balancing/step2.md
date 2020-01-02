## Weighted round robin

The optional load balancing weight of the upstream host; at least 1. Envoy uses the load balancing weight in some of the built in load balancers. The load balancing weight for an endpoint is divided by the sum of the weights of all endpoints in the endpoint’s locality to produce a percentage of traffic for the endpoint. This percentage is then further weighted by the endpoint’s locality’s load balancing weight from LocalityLbEndpoints. If unspecified, each host is presumed to have equal weight in a locality.

Open envoy configuration file `envoy/envoy.yaml`{{open}}

Add to envoy's configuration the definition of `load_assignment`
<pre class="file" data-filename="envoy.yaml" data-target="append">
    load_assignment:
      cluster_name: targetCluster
      endpoints:
      - lb_endpoints:
        - endpoint:
            address:
              socket_address:
                address: 172.18.0.3
                port_value: 80
        load_balancing_weight: 90
      - lb_endpoints:
        - endpoint:
            address:
              socket_address:
                address: 172.18.0.4
                port_value: 80
</pre>


Start the envoy proxy with the defined configuration using this command:

```
docker run --name=proxy -d \
  -p 80:10000 \
  -p 9901:9901 \
  -v $(pwd)/envoy/:/etc/envoy \
  envoyproxy/envoy:latest
```{{execute}}

And then start two http servers using this command:
```
docker run -d katacoda/docker-http-server:healthy;
docker run -d katacoda/docker-http-server:healthy;
```{{execute}}
