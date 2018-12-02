Dark releases is the ability to release a new version to production but only make it available to a select number of clusters, such as internal staff or alpha testers. This dark release gives the ability to gain feedback and test changes in a safe way within interferring with most users.

In this case, if the user has a HTTP header set in the result then we'll be routed to a different service.

## Task

Envoy Proxy routes are defined in order, meaning the first route to match is the one that will be used. When applying customisations, such as only requests with a certain header, it's important that this goes before and other routes for the same prefix.

Within having the additional match before other definitions for the prefix, the matching will be ignored.

The configuration below adds a new route for `/service/2` which will only be accessible if the HTTP Header `x-canary-version` has been defined.

<pre class="file" data-filename="envoy.yaml" data-target="insert" data-marker="#TODO Service2">
              - match:
                  prefix: "/service/2"
                  headers:
                  - name: "x-canary-version"
                    exact_match: "service2a"
                route:
                  cluster: service2a
</pre>

Envoy supports a number of different matching patterns for headers that are included within the [HeaderMatcher documentation](https://www.envoyproxy.io/docs/envoy/latest/api-v2/api/v2/route/route.proto.html?highlight=strip%20headers#route-headermatcher).