Now you have to start the upstream cluster. For this we are gonna use one example application:

```
docker run -p 8081:8081 -d -e EDS_SERVER_PORT='8081' gsoria/docker-http-server
```{{execute T2}}

You could test your upstream service executing the following command:
```curl http://localhost:8081 -i```{{execute T3}}

The response of the request should be something similar to:

```
HTTP/1.1 200 OK
Date: Fri, 07 Jun 2019 20:01:27 GMT
Content-Length: 58
Content-Type: text/html; charset=utf-8

a8623918-6c26-41b4-ad4e-d9765e522da4
```

At this point we have the Envoy started, and the upstream cluster started, but they are not connected yet because the *eds_cluster* that we
specified in the configuration is not started yet.

If you inspect the logs of Envoy, you should see errors when Envoy try to fetching the endpoints:

```
[000011][warning][config] [bazel-out/k8-opt/bin/source/common/config/_virtual_includes/http_subscription_lib/common/config/http_subscription_impl.h:101] REST config update failed: fetch failure
```