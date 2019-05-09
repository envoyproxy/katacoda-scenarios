This scenario uses a crafted *nginx.conf* based on the full example from the [NGINX Wiki](https://www.nginx.com/resources/wiki/start/topics/examples/fullexample2/). You can view the configuration in the editor by opening _`nginx.conf`{{open}}_

NGINX configurations generally have three key elements:

1) Configuring the NGINX server, logging structure and Gzip functionality. This is defined globally across all instances.

2) Configuring NGINX to accept requests for _one.example.com_ host on port 8080.

3) Configuring the target location of how to handle traffic to different parts of the URL.

Not all of the configuration will apply to Envoy Proxy, and you won’t need to configure certain aspects.
Envoy Proxy has **four key types** that support the core infrastructure offered by NGINX. The core is:

* **Listeners:** They define how Envoy Proxy accepts incoming requests. At present, Envoy Proxy only supports TCP-based listeners. Once a connection is made, it’s passed on to a set of filters for processing.

* **Filters:** They are part of a pipeline architecture that can process inbound and outbound data. This functionality enables filters such as Gzip which compresses data before sending it to the client.

* **Routers:** These forward traffic to the required destination, defined as a cluster.

* **Clusters:** They define the target endpoint for traffic and the configuration settings.

We'll use these four components to create an Envoy Proxy configuration to match the NGINX configuration defined. Envoy's focus has been on API and dynamic configuration. In this case, the configuration will use static, hardcoded resources as defined by NGINX.