The first part of the `nginx.conf`{{open}} defines some of the NGINX internals should be configured.

## Worker Connections

The configuration below is focused on defining the number of worker processes and connections. This indicates how NGINX will scale to handle demand.

<pre>
worker_processes  2;

events {
  worker_connections   2000;
}
</pre>

Envoy Proxy manages the worker processes and connections differently... TODO: Explain Envoy Process  

## HTTP Configuration

The next block of configuration defines the HTTP settings, such as which mine types are supported, default timeouts and gzip configuration. These aspects are configured via Filters.
