<pre class="file" data-filename="envoy.yaml" data-target="append">
  clusters:
  - name: targetCluster
    connect_timeout: 0.25s
    lb_policy: ROUND_ROBIN
    type: EDS
    refresh_delay: 30s
    eds_cluster_config:
      service_name: localservices
      eds_config:
        path: '/etc/envoy/eds.conf'

</pre>

The important fields are:

* **type**: EDS

* **refresh_delay**: 

* **eds_cluster_config.service_name**: 