With EDS in place, it's possible to scale up the upstream clusters. If we wanted to be able to dynamically add new domains and clusters, the cluster discovery service (CDS) API needs to be implemented. In the following steps, we are configuring CDS and LDS additionally to EDS previously configured.

You need to create a file to put the configuration for the clusters: `cds.conf`{{open}}.

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
  }]
}
</pre>

And also, you need to create a file to put the configuration for the listeners: `lds.conf`{{open}}.

<pre class="file" data-filename="lds.conf" data-target="replace">
{
    "version_info": "0",
    "resources": [{
            "@type": "type.googleapis.com/envoy.api.v2.Listener",
            "name": "listener_0",
            "address": {
                "socket_address": {
                    "address": "0.0.0.0",
                    "port_value": 10000
                }
            },
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
                                                        "cluster": "targetCluster"
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
    }]
}
</pre>

With the externalized the configuration of clusters and listeners, we need to modify our Envoy configuration to make reference to these files. This can be accomplish changing all the `static_resources` for `dynamic_resources`.

Open the envoy configuration file `envoy1.yaml`{{open}}.

<pre class="file" data-filename="envoy1.yaml" data-target="append">
dynamic_resources:
  cds_config:
    path: "/etc/envoy/cds.conf"
  lds_config:
    path: "/etc/envoy/lds.conf"
</pre>

After that, launch the container with the following command:
Note: to avoid port conflicts, we exposed the ports with offset 1.

```
docker run --name=proxy-eds-cds-lds-filebased -d \
    -p 9902:9901 \
    -p 81:10000 \
    -v /root/:/etc/envoy \
    -v /root/envoy1.yaml:/etc/envoy/envoy.yaml \
    envoyproxy/envoy:latest
```{{execute}}

Execute the following command:
`curl localhost:81`{{execute}}

Based on how Docker handles fle inode tracking, sometimes the inotify filesystem change isn't triggered and detected. Force the change with the command `mv cds.conf tmp; mv tmp cds.conf`{{execute}}


