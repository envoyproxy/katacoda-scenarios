## Restart Proxy

`docker rm -f proxy1; docker run -d --name proxy1 -p 80:8080 -v /root/:/etc/envoy envoyproxy/envoy`{{execute}}

`docker ps -q | xargs -n 1 docker inspect --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}} {{ .Config.Hostname }} {{ .Config.Image }}' | sed 's/ \// /'`{{execute T1}}

## Testing

`for i in {1..10}; do curl -s http://localhost/service/3; done`{{execute}}