## Delete Endpoint

Ok, so now we've dynamically added in an endpoint...lets remove it by the SDS server's custom API and emptying out its hosts: []

```
curl -X PUT --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
  "hosts": [  ]
}' http://localhost:8080/edsservice/myservice
```{{execute}}

Now is you try to send a request to Envoy, you should see no healthy upstream message from envoy:

```curl -v https://localhost:10000/```{{execute}}

```
< HTTP/1.1 503 Service Unavailable
< content-length: 19
< content-type: text/plain
< date: Mon, 30 Apr 2018 06:23:40 GMT
< server: envoy
<
* Connection #0 to host localhost left intact
no healthy upstream
```
