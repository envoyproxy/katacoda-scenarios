The 

`docker run -it --name proxy1 -p 80:8080 -p 443:8443 -p 8001:8001 -v /root/:/etc/envoy/ envoyproxy/envoy`{{execute T2}}


`docker run -d katacoda/docker-http-server; docker run -d katacoda/docker-http-server;`{{execute T1}}


`curl -k -H "Host: example.com" http://localhost -i`{{execute}}


`curl -k -H "Host: example.com" https://localhost/service/1 -i`{{execute}}

`curl -k -H "Host: example.com" https://localhost/service/2 -i`{{execute}}