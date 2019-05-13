With the additional configuration in place, Envoy can be started.

In this case, the Proxy is exposed on both Port 80 for the HTTP traffic, and 443 for HTTPS. It's also exposing the admin dashboard on 8001 allowing you to view the dashboard information about the certificates.

## Start Envoy

`docker run -it --name proxy1 -p 80:8080 -p 443:8443 -p 8001:8001 -v /root/:/etc/envoy/ envoyproxy/envoy`{{execute T2}}

All the HTTPS and TLS termination is handled via Envoy Proxy meaning the application doesn't need to be modified. To start a series of HTTP servers to handle the incoming requests run the following command:

`docker run -d katacoda/docker-http-server; docker run -d katacoda/docker-http-server;`{{execute T1}}

## Testing Configuration

With the Proxy started, it's possible to test the configuration.

First, if you issue an HTTP request the Proxy should return a redirect response to the HTTPS version due to your configuration flag.

`curl -H "Host: example.com" http://localhost -i`{{execute T1}}

You could see the response that indicates the redirect you configured.
```
HTTP/1.1 301 Moved Permanently
location: https://example.com/
```

HTTPS requests will be handled according to your configuration. Try the following requests:

`curl -k -H "Host: example.com" https://localhost/service/1 -i`{{execute T1}}

`curl -k -H "Host: example.com" https://localhost/service/2 -i`{{execute T1}}

Note, without `-k` argument, cURL will respond with an error due to the self-signed certificate:

`curl -H "Host: example.com" https://localhost/service/2 -i`{{execute T1}}

## Verify Certificate

Using OpenSSL CLI it's possible to view the certificate returned from a server. This will allow us to verify the correct certificate is being returned from Envoy:

`echo | openssl s_client -showcerts -servername example.com -connect localhost:443 2>/dev/null | openssl x509 -inform pem -noout -text`{{execute T1}}


## Dashboard

The dashboard returns information about the certificates defined and their age. More information can be discovered at https://[[HOST_SUBDOMAIN]]-8001-[[KATACODA_HOST]].environments.katacoda.com/certs