<pre class="file" data-filename="envoy.yaml">
admin:
  access_log_path: /tmp/admin_access.log
  address:
    socket_address: { address: 0.0.0.0, port_value: 9090 }
</pre>

`docker rm -f proxy1; docker run --name proxy1 -p 80:8080 -p 9090:9090 --user 1000:1000 -v /root/envoy/envoy.yaml:/etc/envoy/envoy.yaml envoyproxy/envoy`{{execute interupt T2}}
