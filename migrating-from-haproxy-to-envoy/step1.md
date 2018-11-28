An example HA Proxy configuration can be viewed in the editor by opening `haproxy.cfg`{{open}}. HA Proxy configurations generally have three key elements.

1) Configuring the HA Proxy Server, logging structure and gzip functionality. This is defined globally across all instances.

2) Configure HA Proxy to accept incoming requests.

3) Configure the location of how to process the traffic.

Not all of the configuration will apply to Envoy Proxy with certain aspects not being required due to different architecture and decisions. Envoy Proxy has four key types that support the core infrastructure offered by HA Proxy. The core is:

* **Listeners:** Listeners define how Envoy Proxy accepts incoming requests. At present, Envoy Proxy only supports TCP based listeners. Once a connection is made, it is based on a set of filters for processing.

* **Filters:** Filters are part of a pipeline architecture that can process inbound and outbound data. This functionality enables filters such as GZip which compresses data before sending it to the client.

* **Routers:** Routers forward traffic to the required destination, defined as a cluster.

* **Clusters:** Clusters define the target endpoint for traffic and the configuration settings.

We'll use these four components to create an Envoy Proxy configuration to match the HA Proxy configuration defined. Envoy's focus has been on API and dynamic configuration. In this case, the configuration will use static, hardcoded, resources as defined by HA Proxy.