To rollout all the traffic to the new service, the weight of the cluster should be changed to 100. The old cluster definition can be removed. 

<pre class="file" data-target="clipboard">
              - match:
                  prefix: "/service/3"
                route:
                  weighted_clusters:
                    clusters:
                    - name: service3b
                      weight: 100
</pre>

## Restart Proxy

As before, the proxy needs to be restarted to pick up the new changes. 

`docker rm -f proxy1; docker run -d --name proxy1 -p 80:8080 -v /root/:/etc/envoy envoyproxy/envoy`{{execute}}

Now when the endpoint is accessed, all the traffic will me from the `service3b` cluster.

`for i in {1..10}; do curl -s http://localhost/service/3; done`{{execute}}
