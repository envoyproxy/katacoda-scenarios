`curl localhost:10000 -H "Host: one.example.com" -i`{{execute T1}}

```
$ curl localhost:10000 -H "Host: one.example.com" -i
HTTP/1.1 503 Service Unavailable
content-length: 57
content-type: text/plain
date: Tue, 27 Nov 2018 17:31:20 GMT
server: envoy

upstream connect error or disconnect/reset before headers
```