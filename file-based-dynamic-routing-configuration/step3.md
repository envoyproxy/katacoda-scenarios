The contents of `eds.conf`{{open}} is a JSON definition of the same information defined within our static configuration. 

## Task

Create `eds.conf`{{open}} file with the following content:

<pre class="file" data-filename="eds.conf" data-target="replace">
{
  "version_info": "0",
  "resources": [{
    "@type": "type.googleapis.com/envoy.api.v2.ClusterLoadAssignment",
    "cluster_name": "localservices",
    "endpoints": [{
      "lb_endpoints": [{
        "endpoint": {
          "address": {
            "socket_address": {
              "address": "172.18.0.3",
              "port_value": 80
            }
          }
        }
      }]
    }]
  }]
}
</pre>

This defines a single endpoint at `172.18.0.3`.