In order to Envoy fetch the endpoints, we need to start the eds_server. For this example, we will use an [example eds_server](https://github.com/salrashid123/envoy_discovery) implemented in python.

Now you have to start the upstream cluster. For this we are gonna use one example python application:

```
source env/bin/activate
pip install -r requirements.txt
python main.py
```{{execute T4}}

You should see the following output on SDS stdout indicating an inbound Envoy discovery request:

```
Inbound v2 request for discovery.  POST payload: {u'node': {u'build_version': u'fd44fd6051f5d1de3b020d0e03685c24075ba388/1.6.0-dev/Clean/RELEASE', u'cluster': u'mycluster', u'id': u'test-id'}, u'resource_names': [u'myservice']}
127.0.0.1 - - [29/Apr/2018 22:59:04] "POST /v2/discovery:endpoints HTTP/1.1" 200 -
```

Then on the envoy proxy stdout, something like:

```
[157796][debug][config] bazel-out/k8-opt/bin/source/common/config/_virtual_includes/http_subscription_lib/common/config/http_subscription_impl.h:67] Sending REST request for /v2/discovery:endpoints
[157796][debug][router] source/common/router/router.cc:250] [C0][S636378528925215024] cluster 'eds_cluster' match for URL '/v2/discovery:endpoints'
[157796][debug][router] source/common/router/router.cc:298] [C0][S636378528925215024]   ':method':'POST'
[157796][debug][router] source/common/router/router.cc:298] [C0][S636378528925215024]   ':path':'/v2/discovery:endpoints'
[157796][debug][router] source/common/router/router.cc:298] [C0][S636378528925215024]   ':authority':'eds_cluster'
...
[157796][debug][client] source/common/http/codec_client.cc:52] [C2] connected
[157796][debug][pool] source/common/http/http1/conn_pool.cc:225] [C2] attaching to next request
...
[157796][debug][client] source/common/http/codec_client.cc:81] [C2] response complete
[157796][debug][pool] source/common/http/http1/conn_pool.cc:200] [C2] response complete
...
[157796][debug][pool] source/common/http/http1/conn_pool.cc:115] [C2] client disconnected
```

You can verify that envoy doesn't know anything about this endpoint by attempting to connect through to it:

```curl -v http://localhost```{{execute T4}}
