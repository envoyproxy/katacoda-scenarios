The final part of the HA Proxy configures where to store log files and the user who owns the process.

<pre>
global
        log /dev/log    local0
        log /dev/log    local1 notice
        user haproxy
        group haproxy
        daemon

defaults
        log     global
        mode    http
        option  httplog
        option  dontlognull
</pre>

The logs for Envoy Proxy follow a cloud native approach, where the logs are outputted to stdout and stderr. Users are defined when the process launches, discussed in the next step.