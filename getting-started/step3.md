Envoy provides an administration view, allowing you to view configuration, stats, logs and other internal Envoy data.

At the end of the proxy, an Admin section was added. The admin can be defined by adding an additional resource definition. The definition defines the port which the admin view is available on. The port should not conflict with other listener configurations.

<pre class="file">admin:
  access_log_path: /tmp/admin_access.log
  address:
    socket_address: { address: 0.0.0.0, port_value: 9901 }
</pre>

## Start Admin

This Docker Container also exposes the Admin port to the outside world. The resource configuration above will make the admin view available to the public and should only be used for demonstration purposes, see the documentation on how to secure the admin portal.

`docker run --name=proxy-with-admin -d \
    -p 9901:9901 \
    -p 10000:10000 \
    -v $(pwd)/envoy/envoy.yaml:/etc/envoy/envoy.yaml \
    envoyproxy/envoy:latest`{{execute}}

The dashboard is now available at https://[[HOST_SUBDOMAIN]]-9901-[[KATACODA_HOST]].environments.katacoda.com/

> "The administration interface in its current form both allows destructive operations to be performed (e.g., shutting down the server) as well as potentially exposes private information (e.g., stats, cluster names, cert info, etc.). It is critical that access to the administration interface is only allowed via a secure network" **[Envoy Documentation](https://www.envoyproxy.io/docs/envoy/latest/operations/admin)**
