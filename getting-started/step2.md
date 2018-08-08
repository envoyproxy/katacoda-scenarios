With the configuration created, when launching the container it needs to be provided to Envoy. With Docker, this is done via a Volume Mount to */etc/envoy/envoy.yaml*.

## Start Envoy 

Below will launch the Proxy, bound to port 80.

`docker run --name=proxy -d \
  -p 80:10000 \
  -v $(pwd)/envoy/envoy.yaml:/etc/envoy/envoy.yaml \
  envoyproxy/envoy:latest`{{execute}}

## View Envoy
Once started, you should be able to send HTTP requests to Port 80 with `curl localhost`{{execute}}

You can view this via your local browser with the URL https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/
As you will see the request is proxied to Google.com and you should see the Google homepage without the URL changing.