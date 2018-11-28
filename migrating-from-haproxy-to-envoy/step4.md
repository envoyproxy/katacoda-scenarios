The final part of the HAProxy configures where to store log files and the user who owns the process.

<pre class="file">
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

The logs for Envoy Proxy follow a cloud native approach, where the application logs are outputted to stdout and stderr. 

User access logs are disabled by default and need to be configured. To enable access logs for HTTP requests, include an `access_log` configuration for the HTTP Connection Manager. The path can be either a device, such as stdout, or a file on disk depending on your requirements.

<pre class="file" data-target="clipboard">
access_log:
- name: envoy.file_access_log
  config:
    path: "/dev/stdout"
</pre>

For example:
<pre class="file">
      - name: envoy.http_connection_manager
        config:
          codec_type: auto
          stat_prefix: ingress_http
          access_log:
          - name: envoy.file_access_log
            config:
              path: "/dev/stdout"
          route_config:
</pre>


By default, Envoy has a Format String that includes details of the HTTP request.

`[%START_TIME%] "%REQ(:METHOD)% %REQ(X-ENVOY-ORIGINAL-PATH?:PATH)% %PROTOCOL%"
%RESPONSE_CODE% %RESPONSE_FLAGS% %BYTES_RECEIVED% %BYTES_SENT% %DURATION%
%RESP(X-ENVOY-UPSTREAM-SERVICE-TIME)% "%REQ(X-FORWARDED-FOR)%" "%REQ(USER-AGENT)%"
"%REQ(X-REQUEST-ID)%" "%REQ(:AUTHORITY)%" "%UPSTREAM_HOST%"\n`

The result of this Format String is:

`[2018-11-23T04:51:00.281Z] "GET / HTTP/1.1" 200 - 0 58 4 1 "-" "curl/7.47.0" "f21ebd42-6770-4aa5-88d4-e56118165a7d" "one.example.com" "172.18.0.4:80"`

The contents of the output can be customised by setting the format field, for example:

<pre class="file">
access_log:
- name: envoy.file_access_log
  config:
    path: "/dev/stdout"
    format: "[%START_TIME%] "%REQ(:METHOD)% %REQ(X-ENVOY-ORIGINAL-PATH?:PATH)% %PROTOCOL%" %RESPONSE_CODE% %RESP(X-ENVOY-UPSTREAM-SERVICE-TIME)% "%REQ(X-REQUEST-ID)%" "%REQ(:AUTHORITY)%" "%UPSTREAM_HOST%"\n"
</pre>

The log line can also be outputted as JSON by setting the json_format field, for example:
<pre class="file">
access_log:
- name: envoy.file_access_log
  config:
    path: "/dev/stdout"
    json_format: {"protocol": "%PROTOCOL%", "duration": "%DURATION%", "request_method": "%REQ(:METHOD)%"}
</pre>

More information on Envoy's logging approach can be found at https://www.envoyproxy.io/docs/envoy/latest/configuration/access_log#config-access-log-format-dictionaries

Logging isn't the only way to gain visibility into production with Envoy Proxy. Built into the platform is advanced tracing and metrics capabilities. You can find out more in the [tracing documentation](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/tracing) or via the [Interactive Tracing Scenario]().