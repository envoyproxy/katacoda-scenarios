To secure traffic, the addition of a **tls_context** is required as a filter. This TLS context provides the ability to specify a collection of certificates for the domains. 

In this case, the certificates are our self-signed generated in the first step. 

//TODO: Move to using Place Holders
<pre>
tls_context:
  common_tls_context:
    tls_certificates:
      - certificate_chain:
          filename: "/etc/certs/example-com.crt"
        private_key:
          filename: "/etc/certs/example-com.key"
</pre>

The completed example can be viewed at `envoy-completed.yaml`{{open}}