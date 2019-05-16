As part of the definition for the routing, a ***weighted_clusters*** can be defined. This takes in a series of cluster targets for the endpoint, each with a defined weight. The weight indicates what share of the traffic the cluster should receive.

In this example, we've verified the service is working using a private deployment that was accessed by header-based routing. The intention is now to start rolling out production traffic to the new service.

By rolling out traffic in a phased motion it's possible to check that no new bugs, performance degradations or other problems have been introduced.

## Task

Replace the string _#TODO Service3_ within the `envoy.yaml`{{open}} file with the configuration snippet below.

<pre class="file" data-filename="envoy.yaml" data-target="insert" data-marker="#TODO Service3">
- match:
                  prefix: "/service/3"
                route:
                  weighted_clusters:
                    clusters:
                    - name: service3a
                      weight: 80
                    - name: service3b
                      weight: 20
</pre>

The snippet adds the new route with an 80/20 weighting between two different clusters. Within the configuration, the clusters for **service3a** and **service3b** have already been defined. The results should be responses indicating a V1 from `curl 172.18.0.6`{{execute}} and V2 response from `curl 172.18.0.7`{{execute}}.