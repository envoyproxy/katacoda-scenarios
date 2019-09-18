In case that you perform a request using a non existing domain:

`curl localhost -H "Host: missing.example.com" -i`{{execute T1}}

You would have an error similar to:

```
$ curl localhost -H "Host: missing.example.com" -i
HTTP/1.1 404 Not Found
date: Tue, 27 Nov 2018 17:32:06 GMT
server: envoy
content-length: 0
```

In the case of this error, check your envoy configuration.
