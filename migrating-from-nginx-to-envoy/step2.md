The first part of the _`nginx.conf`{{open}}_ defines some of the NGINX internals that should be configured.

## Worker Connections

The configuration below focuses on defining the number of worker processes and connections. This indicates how NGINX will scale to handle demand.

<pre>
worker_processes  2;

events {
  worker_connections   2000;
}
</pre>

Envoy Proxy manages the worker processes and connections differently.

Envoy spawns a worker thread for every hardware thread in the system. Each worker thread runs a non-blocking event loop that is responsible for

1. Listening on every listener
2. Accepting new connections
3. Instantiating a filter stack for the connection
4. Processing all IO for the lifetime of the connection.

All further handling of the connection is entirely processed within the worker thread, including any forwarding behaviour.

All connection pools in Envoy are per worker thread. Though HTTP/2 connection pools only make a single connection to each upstream host at a time, if there are four workers, there will be four HTTP/2 connections per upstream host at steady state. By keeping everything within a single worker thread, almost all the code can be written without locks and as if it is single threaded.  Having more workers than necessary will waste memory, create more idle connections, and lead to a lower connection pool hit rate.

For more information, visit the [Envoy Proxy blog](https://blog.envoyproxy.io/envoy-threading-model-a8d44b922310).

## HTTP Configuration

The next block of NGINX configuration defines the HTTP settings, such as:
* Which mime types are supported
* Default timeouts
* Gzip configuration

You can configure these aspects via Filters within Envoy Proxy, which we will discuss in a later step.