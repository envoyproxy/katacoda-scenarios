Within NGINX, the upstream configuration defines the set of target servers that will handle the traffic. In this case, two clusters have been assigned.

<pre class="file">
  upstream targetCluster {
    172.18.0.4:80;
    172.18.0.5:80;
  }

</pre>

## Envoy Cluster

The equiviant of upstream is defined as Clusters. In this case, the hosts have been defined. The way the hosts are accessed, such as the timeouts are defined as the cluster configuration. This allows finer gain control over aspects such as timeouts and load balancing.

<pre class="file" data-filename="envoy.yaml">
  clusters:
  - name: targetCluster
    connect_timeout: 0.25s
    type: STRICT_DNS
    dns_lookup_family: V4_ONLY
    lb_policy: ROUND_ROBIN
    hosts: [
      { socket_address: { address: 172.18.0.3, port_value: 80 }},
      { socket_address: { address: 172.18.0.4, port_value: 80 }}
    ]
</pre>

The type **STRICT_DNS** refers to...