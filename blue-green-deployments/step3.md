With the configuration created, start Envoy with the command below:

`docker run -d --name proxy1 -p 80:8080 -v /root/:/etc/envoy envoyproxy/envoy`{{execute T1}}

The configuration has hardcoded a series of IP addresses that are the destinations of the traffic. The command below will start a series of HTTP servers. Some of the HTTP servers will return different responses based on their version. Click the command to launch it within the terminal.

`docker run -d katacoda/docker-http-server:v1; docker run -d katacoda/docker-http-server:v1; docker run -d katacoda/docker-http-server:v2; docker run -d katacoda/docker-http-server:v2; docker run -d katacoda/docker-http-server:v3`{{execute T1}}

It's possible to compare the Envoy cluster configuration with the IP address and Container Hostname with the command:

`docker ps -q | xargs -n 1 docker inspect --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}} {{ .Config.Hostname }} {{ .Config.Image }}' | sed 's/ \// /'`{{execute T1}}

## Test via HTTP Headers

With everything deployed, it's possible to test the configuration using cURL. 

Sending requests to the `/service/2` endpoint, it will return a V1 response. 

`curl http://localhost/service/2`{{execute}}

If we define a HTTP Header that matches the configuration, then the V2 response should be returned.

`curl -H "x-canary-version: service2a" http://localhost/service/2`{{execute}}

Our traffic is now being controlled and varied based on the request, giving us the ability to perform private releases.