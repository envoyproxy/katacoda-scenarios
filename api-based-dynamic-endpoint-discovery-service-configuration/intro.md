When an upstream cluster is defined in the configuration, Envoy needs to know how to resolve the members of the cluster. This is known as service discovery.
The endpoint discovery service (EDS) is a xDS management server based on gRPC or REST-JSON API server used by Envoy to fetch cluster members.

In this scenario, you will learn how configure the discovery of endpoints using a REST-JSON API. This scenario will explain:

- Configuration of EDS

- Add endpoints to the cluster using the REST API

- Delete endpoints using the REST API