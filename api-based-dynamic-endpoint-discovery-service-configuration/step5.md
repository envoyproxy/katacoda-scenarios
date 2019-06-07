Now we're ready to add an upstream service configuration to the EDS server.

## Create Endpoint
Since we defined the service as *myservice* in the envoy_config.yaml, we can need to register an endpoint against it:

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

## Check client connectivity via envoy
Since we already started the upstream service above, you can connect to it via envoy:

```curl -i http://localhost:10000```{{execute T5}}

You should see something similar to:

```
HTTP/1.1 200 OK
content-type: text/html; charset=utf-8
content-length: 36
server: envoy
date: Fri, 07 Jun 2019 04:57:54 GMT
x-envoy-upstream-service-time: 1
```

## Add new nodes to cluster
Now you can run more nodes in your upstream cluster and register dynamically calling to the API.
With this command you will add 4 more nodes to the upstream cluster:

```
for i in { 8082 8083 8084 8085 }
  do
      source env/bin/activate
      nohup python server.py -p $i &
      sleep .5
done
```{{execute T5}}

You can verify the new nodes and the corresponding ports with the following command:

```
netstat -tnlp
```{{execute T5}}

And these nodes should be registered in the eds_cluster:

```
curl -X PUT --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
    "hosts": [
        {
        "ip_address": "0.0.0.0",
        "port": 8081,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        },
        {
        "ip_address": "0.0.0.0",
        "port": 8082,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        },
        {
        "ip_address": "0.0.0.0",
        "port": 8083,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        },
        {
        "ip_address": "0.0.0.0",
        "port": 8084,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        },
        {
        "ip_address": "0.0.0.0",
        "port": 8085,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        }
    ]
    }' http://localhost:8080/edsservice/myservice
```{{execute T5}}


Now you can verify that the traffic is balanced with all the nodes registered with the following command:

`while true; do curl http://localhost:10000; sleep .5; printf '\n'; done`{{execute T6}}

You will see different responses according to the service that processed the request.