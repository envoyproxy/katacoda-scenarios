An initial outline of the Envoy configuration required is available at `envoy.yaml`{{open}}

The first change required is to add a cluster configuration, with type EDS, and indicate that in eds_config should be using the REST API:

<pre class="file" data-filename="envoy.yaml" data-target="append">
  clusters:
  - name: targetCluster
    type: EDS
    connect_timeout: 0.25s
    eds_cluster_config:
      service_name: myservice
      eds_config:
        api_config_source:
          api_type: REST
          cluster_names: [eds_cluster]
          refresh_delay: 5s
</pre>

Note: *api_type* is set to v2 REST endpoint. If you want to swtich to v1 simply use *api_type: REST_LEGACY*

After that you need to define how ***eds_cluster*** are resolved. For this example we are gonna use an static configuration:

<pre class="file" data-filename="envoy.yaml" data-target="append">
  - name: eds_cluster
    type: STATIC
    connect_timeout: 0.25s
    hosts: [{ socket_address: { address: 0.0.0.0, port_value: 8080 }}]
</pre>

## Task

Launch Envoy with the following command:

```
docker run --name=api-eds -d \
    --network host \
    -p 9901:9901 \
    -p 80:10000 \
    -v /root/:/etc/envoy \
    envoyproxy/envoy:latest
```{{execute}}
