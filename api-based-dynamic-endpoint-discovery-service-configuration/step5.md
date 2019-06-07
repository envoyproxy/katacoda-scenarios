Now we're ready to add an upstream service configuration to the EDS server.

## Create Endpoint
Since we defined the service as myservice in the envoy_config.yaml, we can need to register an endpoint against it:

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
  "hosts": [
    {
      "ip_address": "0.0.0.0",
      "port": 8081,
      "tags": {
        "az": "us-central1-a",
        "canary": false,
        "load_balancing_weight": 50
      }
    }
  ]
}' http://localhost:8080/edsservice/myservice
```{{execute T5}}

What this will do is set some endpoints for myservice. Now, envoy will query SDS for membership so on the next poll, you'll see some lines like:

```
[159226][debug][upstream] source/common/upstream/eds.cc:105] EDS hosts changed for cluster: service_backend (0) priority 0
[159231][debug][upstream] source/common/upstream/cluster_manager_impl.cc:642] membership update for TLS cluster service_backend
[159226][debug][upstream] source/common/upstream/cluster_manager_impl.cc:642] membership update for TLS cluster service_backend
[159233][debug][upstream] source/common/upstream/cluster_manager_impl.cc:642] membership update for TLS cluster service_backend
[159234][debug][upstream] source/common/upstream/cluster_manager_impl.cc:642] membership update for TLS cluster service_backend
[159232][debug][upstream] source/common/upstream/cluster_manager_impl.cc:642] membership update for TLS cluster service_backend
[159226][debug][pool] source/common/http/http1/conn_pool.cc:200] [C7] response complete
[159226][debug][pool] source/common/http/http1/conn_pool.cc:220] [C7] moving to ready
```

Check client connectivity via envoy
Since we already started the upstream service above, you can connect to it via envoy:

```curl -v http://localhost```{{execute T5}}

You should see something similar to:

```
< HTTP/1.1 200 OK
< content-type: text/html; charset=utf-8
< content-length: 36
< server: envoy
< date: Mon, 30 Apr 2018 06:21:43 GMT
< x-envoy-upstream-service-time: 3
<
* Connection #0 to host localhost left intact
40b9bc6f-77b8-49b7-b939-1871507b0fcc
```