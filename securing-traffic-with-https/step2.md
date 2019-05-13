To secure HTTP traffic the addition of a ***`tls_context`*** is required as a filter. The TLS context provides the ability to specify a collection of certificates for the domains configured within Envoy Proxy. When an HTTPS request is being processed, the matching certificate will be used.

In this case, the certificates are our self-signed generated in the first step.

## Add TLS Context to HTTPS Listener

Open the `envoy.yaml`{{open}} configuration file. It contains an outline of the required HTTPS support. It has two listeners configured, one on port 8080 for HTTP traffic and another on 8443 for HTTPS traffic.

The HTTPS listener has HTTP Connection Manager defined that will proxy incoming requests for `/service/1` and `/service/2` endpoints. This needs to be extended to include the required ***`tls_context`***  as shown below.

<pre class="file" data-filename="envoy.yaml" data-target="insert" data-marker="#TODO:TLS-Context">
tls_context:
        common_tls_context:
          tls_certificates:
            - certificate_chain:
                filename: "/etc/envoy/certs/example-com.crt"
              private_key:
                filename: "/etc/envoy/certs/example-com.key"
</pre>

Within the context, the certificate and key generated are defined. If we had multiple domains each with their own certificate, then multiple certificate chains would be defined.

The completed configuration can be viewed at `envoy-completed.yaml`{{open}}.