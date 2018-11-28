For testing purposes the command below will generate a self-signed certificate for the domain example.com. This self-signed will cause warning messages but ideal for testing configuration locally.

openssl req  -nodes -new -x509  -keyout server.key -out server.cert

`mkdir certs; cd certs; openssl req -nodes -new -x509 -keyout example-com.key -out example-com.crt -days 365; cd -`{{execute}}

Certain information is required to generate a certificate. In this example, the most important example is **setting the Common Name to *example.com***. Here are some example details:

```
Country Name (2 letter code) []:UK
State or Province Name (full name) []:UK
Locality Name (eg, city) []:London
Organization Name (eg, company) []:Katacoda
Organizational Unit Name (eg, section) []:Envoy
Common Name (eg, fully qualified host name) []:example.com
Email Address []:founders@example.com
```

Envoy Proxy requires both the key and crt file.

When deploying into production, you will need certificates generated for your site from a service such as [Let’s Encrypt](https://letsencrypt.org).