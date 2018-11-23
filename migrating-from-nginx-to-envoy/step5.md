Within NGINX, the upstream configuration defines the set of target servers that will handle the traffic. In this case, two clusters have been assigned.

<pre>
  upstream targetCluster {
    172.17.0.4:3000;
    172.17.0.5:3000;
  }

</pre>

## Envoy Cluster

The equiviant of upstream is defined as Clusters. In this case, the hosts have been defined. The way the hosts are accessed, such as the timeouts are defined as the cluster configuration. This allows finer gain control over aspects such as timeouts and load balancing.

<pre data-filename="envoy.yaml">
  clusters:
  - name: targetCluster
    connect_timeout: 0.25s
    type: LOGICAL_DNS
    dns_lookup_family: V4_ONLY
    lb_policy: ROUND_ROBIN
    hosts: [
      { socket_address: { address: 172.17.0.4, port_value: 3000 }},
      { socket_address: { address: 172.17.0.5, port_value: 3000 }},
    ]
</pre>