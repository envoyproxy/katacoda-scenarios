This scenario uses a crafted nginx.conf based on the full example from the [NGINX wiki](https://www.nginx.com/resources/wiki/start/topics/examples/fullexample2/). The configuration can be viewed in the editor by opening `nginx.conf`{{open}}

NGINX configurations generally have three key elements.

1) Configuring the NGINX Server, logging structure and gzip functionality. This is defined globally across all instances.

2) Configure NGINX to accept requests for _one.example.com_ host on port 8080.

3) Configure the target location of how to handle traffic to different parts of the URL.

Not all of the configuration will apply to Envoy Proxy, and certain aspects will not be required to be configured. Envoy Proxy has four key types that support the core infrastructure offered by NGINX. The core is:

* **Listeners:** Listeners define how Envoy Proxy accepts incoming requests. At present, Envoy Proxy only supports TCP based listeners. Once a connection is made, it is passed on to a set of filters for processing.

* **Filters:** Filters are part of a pipeline architecture that can process inbound and outbound data. This functionality enables filters such as GZip which compresses data before sending it to the client.

* **Routers:** Routers forward traffic to the required destination, defined as a cluster.

* **Clusters:** Clusters define the target endpoint for traffic and the configuration settings.

We'll use these four components to create an Envoy Proxy configuration to match the NGINX configuration defined. Envoy's focus has been on API and dynamic configuration. In this case, the configuration will use static, hardcoded, resources as defined by NGINX.