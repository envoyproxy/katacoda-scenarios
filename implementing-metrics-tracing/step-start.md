An initial envoy configuration file has been created at
`envoy.yaml`{{open}}

In this file, it is defined that the server will run with a listener using all network interfaces in port 10000.

```
- name: listener_0
  address:
    socket_address: { address: 0.0.0.0, port_value: 10000 }
```

Also this configuration defines two nodes in `targetCluster`

```
hosts: [
  { socket_address: { address: 172.18.0.3, port_value: 80 }},
  { socket_address: { address: 172.18.0.4, port_value: 80 }}
]```

Start the envoy proxy with the defined configuration using this command:

```
docker run --name=proxy -d \
  -p 80:10000 \
  -p 9901:9901 \
  -v $(pwd)/envoy/:/etc/envoy \
  envoyproxy/envoy:latest
```{{execute}}

And then start two healthy http servers using this command:
```
docker run -d katacoda/docker-http-server:healthy;
docker run -d katacoda/docker-http-server:healthy;
```{{execute}}

Check if the nodes are running with this command:

```curl 172.18.0.3:80; curl 172.18.0.4:80```{{execute}}

You should get an answer similar to

```
<h1>A healthy request was processed by host: dfe3613cc3da</h1>
<h1>A healthy request was processed by host: 6db2061eb74a</h1>
```

And you can request through envoy

```curl localhost:80```{{execute}}

Envoy is answering the request and balancing between the two nodes with a `ROUND_ROBIN` strategy according to our configuration.

Also, you can test this via your local browser with the URL https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/
