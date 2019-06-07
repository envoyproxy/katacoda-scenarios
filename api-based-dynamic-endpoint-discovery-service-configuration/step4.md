In order to Envoy fetch the endpoints, we need to start the *eds_cluster*. For this example, we will use an [example eds_server](https://github.com/salrashid123/envoy_discovery) implemented in python.

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
You can verify that envoy doesn't know anything about this endpoint by attempting to connect through to it:

```curl -v http://localhost:10000```{{execute T5}}
