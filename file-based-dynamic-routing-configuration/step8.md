With the configuration based on CDS, LDS and EDS, we can dynamically add a new cluster.

Open the file `cds.conf`{{open}}.

## Add new cluster

We'll call this new cluster `newTargetCluster`. Replace the configuration with the following to add a new cluster.

<pre class="file" data-filename="cds.conf" data-target="replace">
{
  "version_info": "0",
  "resources": [{
      "@type": "type.googleapis.com/envoy.api.v2.Cluster",
      "name": "targetCluster",
			"connect_timeout": "0.25s",
			"lb_policy": "ROUND_ROBIN",
			"type": "EDS",
			"eds_cluster_config": {
				"service_name": "localservices",
				"eds_config": {
					"path": "/etc/envoy/eds.conf"
				}
			}
  },
  {
      "@type": "type.googleapis.com/envoy.api.v2.Cluster",
      "name": "newTargetCluster",
			"connect_timeout": "0.25s",
			"lb_policy": "ROUND_ROBIN",
			"type": "EDS",
			"eds_cluster_config": {
				"service_name": "localservices",
				"eds_config": {
					"path": "/etc/envoy/eds1.conf"
				}
			}
  }]
}
</pre>

You also need to create the `eds_cluster_config` file for this new cluster.
Create the file `eds1.conf`{{open}} with this content:

<pre class="file" data-filename="eds1.conf" data-target="replace">
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
              "address": "172.18.0.6",
              "port_value": 80
            }
          }
        }
      },
	    {
        "endpoint": {
          "address": {
            "socket_address": {
              "address": "172.18.0.7",
              "port_value": 80
            }
          }
        }
      }]
    }]
  }]
}
</pre>

And you can use this new cluster, in the listener that you previously configured. Open the file `lds.conf`{{open}}.
Replace the target cluster with the name of the new cluster `newTargetCluster`.

<pre class="file" data-target="clipboard">
  "route": {
      "cluster": "newTargetCluster"
  }
</pre>

The configuration of `lds.conf` should look like:

<pre class="file">
...
            "filter_chains": [
                {
                    "filters": [
                        {
                            "name": "envoy.http_connection_manager",
                            "config": {
                                "stat_prefix": "ingress_http",
                                "codec_type": "AUTO",
                                "route_config": {
                                    "name": "local_route",
                                    "virtual_hosts": [
                                        {
                                            "name": "local_service",
                                            "domains": [
                                                "*"
                                            ],
                                            "routes": [
                                                {
                                                    "match": {
                                                        "prefix": "/"
                                                    },
                                                    "route": {
                                                       "cluster": "newTargetCluster"
                                                    }
                                                }
                                            ]
                                        }
                                    ]
                                },
                                "http_filters": [
                                    {
                                        "name": "envoy.router"
                                    }
                                ]
                            }
                        }
                    ]
                }
            ]
...
</pre>

Start two HTTP servers to handle the incoming requests for the new cluster
`docker run -d katacoda/docker-http-server; docker run -d katacoda/docker-http-server;`{{execute}}

Based on how Docker handles file inode tracking, sometimes the filesystem change isn't triggered and detected.
Force the change with the command: `mv cds.conf tmp; mv tmp cds.conf; mv lds.conf tmp; mv tmp lds.conf`{{execute}}

Envoy should automatically reload the configuration and add the new cluster. You can try running the following command:
`curl localhost:81`{{execute}}.

You can notice with the response of each request, that the ID of the nodes changes, corresponding to the nodes of `newTargetCluster`.