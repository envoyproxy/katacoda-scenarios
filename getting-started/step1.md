Envoy is configured using a YAML definition defining the proxies behaviour. In this step, we're building a configuring using the Static Configuration API, meaning all the settings are pre-defined within the configuration. 

Envoy supports Dynamic Configuration which enables the settings to be discovered via an external source. 

## Resources

The first line of the Envoy configuration defines the API configuration being used. In this case, we're configuring the Static API meaning the first line should be *static_resources*. Copy the snippet to the editor.

<pre class="file"  data-filename="envoy.yaml" data-target="replace">static_resources:</pre>

## Listeners

The beginning of the configuration defines the *Listeners*. A listener is the networking configuration, such as IP address and Ports, that Envoy listens for requests. Envoy runs inside of a Docker Container meaning it needs to listen on the IP address **0.0.0.0**. In this case, Envoy will listen on port **10000**. 

Below is the configuration to define this setup. Copy the snippet to the editor.

<pre class="file"  data-filename="envoy.yaml" data-target="append">  listeners:
  - name: listener_0
    address:
      socket_address: { address: 0.0.0.0, port_value: 10000 }
</pre>

## Filter Chains and Fliters

With Envoy listening for incoming traffic, the next stage is to define how to process for requests. Each Listener has a set of filters. Different listeners can have a different set of filters. 

In this example, we'll proxy all traffic to Google.com (thanks Google!). The result is we should be able to request the Envoy endpoint and see the Google homepage appear, without the URL changing.

Filtering is defined using *filter_chains*. The aim of each *filter* is to find a match on the inputting request, to match it to the target destination. Copy the snippet to the editor.

<pre class="file"  data-filename="envoy.yaml" data-target="append">    filter_chains:
    - filters:
      - name: envoy.http_connection_manager
        config:
          stat_prefix: ingress_http
          route_config:
            name: local_route
            virtual_hosts:
            - name: local_service
              domains: ["*"]
              routes:
              - match: { prefix: "/" }
                route: { host_rewrite: www.google.com, cluster: service_google }
          http_filters:
          - name: envoy.router
</pre>

The filter become is using *envoy.http_connection_manager*, a built-in filter designed for HTTP connections. The details are as follows:
* stat_prefix: The human-readable prefix to use when emitting statistics for the connection manager. The value *ingress_http* is...

* route_config: The configuration for the route. If the virtual hosts matches then the route is checked. In this example, the route_config matches all incoming HTTP requests, no matter the host domain requested. 

* routes: If the URL prefix is matched then a set of route rules defines what should happen next.

* host_rewrite: Change the inbound Host header for the HTTP request.

* cluster: The name of the cluster which will handle the request. The implementation is defined below.

* http_filters: The filter allows Envoy to adapt and modify the request as it is processed. 

## Clusters

When a filter is matched, it will pass the request to a cluster. The cluster below defines that the host is google.com running over HTTPS. If multiple hosts had been defined, then Envoy would perform a Round Robin strategy. 

Copy the cluster implementation to complete the configuration.

<pre class="file"  data-filename="envoy.yaml" data-target="append">  clusters:
  - name: service_google
    connect_timeout: 0.25s
    type: LOGICAL_DNS
    dns_lookup_family: V4_ONLY
    lb_policy: ROUND_ROBIN
    hosts: [{ socket_address: { address: google.com, port_value: 443 }}]
    tls_context: { sni: www.google.com }
</pre>

This structure defines the boilerplate for Envoy Static Configuration. The listener defines the ports and IP address for Envoy. The listener has a set of filters to match on the incoming requests. Once a request is matched, it will be forwarded to a cluster.

You can view the full configuration on [Github](https://github.com/envoyproxy/envoy/blob/6a578630a8f6189f86bc1e6b4b4d7ebffabadadd/configs/google_com_proxy.v2.yaml)
