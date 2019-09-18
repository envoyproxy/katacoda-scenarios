Make the nodes unhealthy:

`curl 172.18.0.3/unhealthy; curl 172.18.0.4/unhealthy;`{{execute}}

If the upstream host is unresponsive when you run a command like:

`curl localhost -H "Host: one.example.com" -i`{{execute T1}}

You would get a response similar to:

```
$ curl localhost -H "Host: one.example.com" -i
HTTP/1.1 503 Service Unavailable
content-length: 57
content-type: text/plain
date: Tue, 27 Nov 2018 17:31:20 GMT
server: envoy

upstream connect error or disconnect/reset before headers
```

In the case of this error, check if the nodes of your upstream cluster are healthy.
