Since routing rules are applied in order

<pre class="file" data-filename="envoy.yaml" data-target="insert" data-marker="#TODO Service2">
              - match:
                  prefix: "/service/2"
                  headers:
                  - name: "x-canary-version"
                    exact_match: "service2a"
                route:
                  cluster: service2a
</pre>