Let's add another server to our cluster and hot reload Envoy Proxy. Hot Reload means...

<pre class="file" data-target="clipboard">
access_log:
- name: envoy.file_access_log
  config:
    path: "/dev/stdout"
</pre>

`docker run -d katacoda/docker-http-server; docker run -d katacoda/docker-http-server;`{{execute T1}}

`curl -H localhost -i`{{execute T1}}