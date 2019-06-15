What happens if the Envoy loses the connection from EDS server?. Let's try!

First, let's put all the nodes of the upstream cluster again:

```
curl -X PUT --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
    "hosts": [
        {
        "ip_address": "172.18.0.3",
        "port": 8081,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        },
        {
        "ip_address": "172.18.0.5",
        "port": 8082,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        },
        {
        "ip_address": "172.18.0.6",
        "port": 8083,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        },
        {
        "ip_address": "172.18.0.7",
        "port": 8084,
        "tags": {
            "az": "us-central1-a",
            "canary": false,
            "load_balancing_weight": 50
        }
        },
        {
        "ip_address": "172.18.0.8",
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

Verify if the nodes are responding with this command:

`while true; do curl http://localhost; sleep .5; printf '\n'; done`{{execute T6}}

And then, with this command you can stop and delete the container where the EDS server is running:

```
docker ps -a | awk '{ print $1,$2 }' | grep gsoria/eds_server  | awk '{print $1 }' | xargs -I {} docker stop {};
docker ps -a | awk '{ print $1,$2 }' | grep gsoria/eds_server  | awk '{print $1 }' | xargs -I {} docker rm {}
```{{execute T7}}

As you can see in the other terminal, Envoy continues response to the requests even when it's disconnected from EDS server.