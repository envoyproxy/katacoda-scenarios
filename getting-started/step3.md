Envoy provides an administration view, allowing you to view configuration, stats, logs and other internal Envoy data.

The admin can be defined by adding an additional resource definition, where the port for the admin view is defined. The port should not conflict with other Listener configurations.

<pre class="file">admin:
  access_log_path: /tmp/admin_access.log
  address:
    socket_address: { address: 0.0.0.0, port_value: 9901 }
</pre>

## Start Admin

This Docker Container also exposes the admin port to the outside world. The resource configuration above will make the admin view available to the public and should be used for demonstration purposes **only**, see the [documentation](https://www.envoyproxy.io/docs/envoy/latest/operations/admin) on how to secure the admin portal.

To expose the admin portal, run the following command:

`docker run --name=proxy-with-admin -d \
    -p 9901:9901 \
    -p 10000:10000 \
    -v $(pwd)/envoy/envoy.yaml:/etc/envoy/envoy.yaml \
    envoyproxy/envoy:latest`{{execute}}

The dashboard is now available at https://[[HOST_SUBDOMAIN]]-9901-[[KATACODA_HOST]].environments.katacoda.com/

> "The administration interface in its current form both allows destructive operations to be performed (e.g., shutting down the server) as well as potentially exposes private information (e.g., stats, cluster names, cert info, etc.). It is critical that access to the administration interface is only allowed via a secure network" **[Envoy Documentation](https://www.envoyproxy.io/docs/envoy/latest/operations/admin)**
