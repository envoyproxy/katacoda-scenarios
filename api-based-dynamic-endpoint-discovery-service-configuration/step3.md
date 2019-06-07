Now you have to start the upstream cluster. For this we are gonna use one example python application:

```
virtualenv env --python=python2.7
source env/bin/activate
pip install -r requirements.txt
python server.py -p 8081
```{{execute T2}}

You could test your upstream service executing the following command:
```curl http://localhost:8081 -i```{{execute T3}}

The response of the request should be something similar to:

```
HTTP/1.0 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 36
Server: Werkzeug/0.15.4 Python/2.7.12
Date: Fri, 07 Jun 2019 01:43:37 GMT

2823cc11-3140-4944-a921-88ca9a9f2fa3$
```

At this point we have the Envoy started, and the upstream cluster started, but they are not connected yet because the eds_server that we
specify in the configuration is not started yet.

If you see the logs of Envoy, you should see errors when Envoy try to fetching the endpoints:

```
[000011][warning][config] [bazel-out/k8-opt/bin/source/common/config/_virtual_includes/http_subscription_lib/common/config/http_subscription_impl.h:101] REST config update failed: fetch failure
```