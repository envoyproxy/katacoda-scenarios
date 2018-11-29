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