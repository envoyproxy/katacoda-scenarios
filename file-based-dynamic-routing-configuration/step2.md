An initial outline of the Envoy configuration required is available at `envoy.yaml`{{open}}

The first changes required is to add a [Node](https://www.envoyproxy.io/docs/envoy/latest/api-v2/api/v2/core/base.proto#core-node). This allows the Envoy node to be identified, potentially allowing for unique configurations to be applied. 

## Task

Prepend the following snippet to the top of the `envoy.yaml`{{open}} file.

<pre class="file" data-filename="envoy.yaml" data-target="prepend">
node:
  id: id_1
  cluster: test
</pre>

The API also has support for additional metadata, such as [locality](https://www.envoyproxy.io/docs/envoy/latest/api-v2/api/v2/core/base.proto#core-locality) for providing region and zone-based information.