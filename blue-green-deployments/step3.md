
## Start Envoy

Start Envoy with the command: 

`docker run -d --name proxy1 -p 80:8080 -v /root/:/etc/envoy envoyproxy/envoy`{{execute T1}}

Start two nodes, both start in a heathy state.

`docker run -d katacoda/docker-http-server:v1; docker run -d katacoda/docker-http-server:v1; docker run -d katacoda/docker-http-server:v2; docker run -d katacoda/docker-http-server:v2; docker run -d katacoda/docker-http-server:v3`{{execute T1}}

`docker ps -q | xargs -n 1 docker inspect --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}} {{ .Config.Hostname }} {{ .Config.Image }}' | sed 's/ \// /'`{{execute T1}}


## Test via HTTP Headers

`curl http://localhost/service/2`{{execute}}

`curl -H "x-canary-version: service2a" http://localhost/service/2`{{execute}}

