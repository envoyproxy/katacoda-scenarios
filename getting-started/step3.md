<pre class="file"  data-filename="envoy.yaml" data-target="clipboard">admin:
  access_log_path: /tmp/admin_access.log
  address:
    socket_address: { address: 127.0.0.1, port_value: 9901 }
</pre>

`docker run --name=proxy-secure -d -p 10001:10000 -v $(pwd)/envoy/envoy.yaml:/etc/envoy/envoy.yaml envoyproxy/envoy:latest`{{execute}}