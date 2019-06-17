In the previous scenarios we've defined the static configuration and dynamic configuration using files. In this scenario we will cover another type of dynamic configuration: API dynamic configuration.

The endpoint discovery service is a xDS management server based on gRPC or REST-JSON API server used by Envoy to fetch cluster members. The cluster members are called **endpoint** in Envoy terminology. For each cluster, Envoy fetch the endpoints from the discovery service. EDS is the preferred service discovery mechanism for a few reasons:

- Envoy has explicit knowledge of each upstream host (vs. routing through a DNS resolved load balancer) and can make more intelligent load balancing decisions.

- Extra attributes carried in the discovery API response for each host inform Envoy of the host’s load balancing weight, canary status, zone, etc. These additional attributes are used globally by the Envoy mesh during load balancing, statistic gathering, etc.

The Envoy project provides reference gRPC implementations of EDS and other discovery services in both [Java](https://github.com/envoyproxy/java-control-plane) and [Go](https://github.com/envoyproxy/go-control-plane).

In the next steps, we'll change our configuration to use ***Endpoint Discovery Service (EDS)*** allowing nodes to be dynamically added based with data coming from the REST-JSON API.