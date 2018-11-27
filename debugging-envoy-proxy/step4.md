`curl localhost:10000 -H "Host: missing.example.com" -i`{{execute T1}}

```
$ curl localhost:10000 -H "Host: missing.example.com" -i
HTTP/1.1 404 Not Found
date: Tue, 27 Nov 2018 17:32:06 GMT
server: envoy
content-length: 0
```