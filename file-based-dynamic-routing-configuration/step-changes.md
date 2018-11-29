<pre class="file" data-filename="eds.conf" data-target="replace">
{
	"version_info": "0",
	"resources": [
		{
			"@type": "type.googleapis.com/envoy.api.v2.ClusterLoadAssignment",
			"cluster_name": "localservices",
			"endpoints": [
				{
					"lb_endpoints": [
						{
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
						}
					]
				}
			]
		}
	]
}
</pre>