In order to Envoy fetch the endpoints, we need to start the *eds_cluster*. For this example, we will use an [example eds_server](https://github.com/salrashid123/envoy_discovery) implemented in python.

Now you have to start the eds_server example with the following command:

```
docker run -p 8080:8080 -d katacoda/eds_server;
```{{execute T4}}

After the command runs and finish pull the image, you should see the following output on SDS stdout indicating an inbound Envoy discovery request:

```
Inbound v2 request for discovery.  POST payload: {u'node': {u'build_version': u'77c59b3e76fcced7f327da3873eb96703eaa49c8/1.9.0-dev/Clean/RELEASE', u'cluster': u'mycluster', u'id': u'test-id'}, u'resource_names': [u'myservice']}
127.0.0.1 - - [07/Jun/2019 05:22:35] "POST /v2/discovery:endpoints HTTP/1.1" 200 -
```
You can verify that envoy doesn't know anything about this endpoint by attempting to connect through to it:

```curl -v http://localhost```{{execute T5}}
