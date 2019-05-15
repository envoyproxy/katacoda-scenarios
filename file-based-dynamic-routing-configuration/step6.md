With the configuration based on EDS, when the services need to be scaled up a new endpoint can be added to the `eds.conf`{{open}}. Envoy will then automatically include the changes.

## Task

Replace the configuration with the following to add a new endpoint to the cluster.

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
      },
	    {
        "endpoint": {
          "address": {
            "socket_address": {
              "address": "172.18.0.4",
              "port_value": 80
            }
          }
        }
      }]
    }]
  }]
}
</pre>

Based on how Docker handles file inode tracking, sometimes the filesystem change isn't triggered and detected. Force the change with the command `mv eds.conf tmp; mv tmp eds.conf`{{execute}}

Envoy should automatically reload the configuration and add the new node into the load balancing rotation `curl localhost`{{execute}}