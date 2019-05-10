An example HA Proxy configuration can be viewed in the editor by opening `haproxy.cfg`{{open}}. HA Proxy configurations generally have four key elements:

1. Configuring the HA Proxy Server, logging structure and process-wide security and performance tunings that affect HAProxy at a low level. This is defined globally across all instances. See `global` section in `haproxy.cfg`.

2. Configuring the default settings apply to all of the `frontend` and `backend` sections that come after it. See `defaults` section in `haproxy.cfg`.

3. Configuring the HA Proxy to accept incoming requests. This section defines the IP addresses and ports that clients can connect to. See `frontend` section in `haproxy.cfg`.

4. Configuring the location of how to process the traffic. It defines a group of servers that will be load balanced and assigned to handle requests. See `backend` section in `haproxy.cfg`.

Not all of the configuration will apply to Envoy Proxy with certain aspects not being required due to different architecture and decisions. Envoy Proxy has **four key types** that support the core infrastructure offered by HA Proxy. The core are:

* **Listeners:** They define how Envoy Proxy accepts incoming requests. At present, Envoy Proxy only supports TCP-based listeners. Once a connection is made, it’s passed on to a set of filters for processing.

* **Filters:** They are part of a pipeline architecture that can process inbound and outbound data. This functionality enables filters such as Gzip which compresses data before sending it to the client.

* **Routers:** These forward traffic to the required destination, defined as a cluster.

* **Clusters:** They define the target endpoint for traffic and the configuration settings.

We'll use these four components to create an Envoy Proxy configuration to match the HA Proxy configuration defined. Envoy's focus has been on API and dynamic configuration. In this case, the configuration will use static, hardcoded, resources as defined by HA Proxy.