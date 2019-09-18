All envoy logs are redirected to stdout and stderr.

To see the container id of your envoy proxy, you can use the following command:
`docker ps`{{execute T1}}

So, using the container id, you can see the logs of your server using a command similar to:
`docker logs 3f94fcd0fdd9`

If you are interested on how the Envoy's docker image is built [this is the link to the docker file](https://github.com/envoyproxy/envoy/blob/master/ci/Dockerfile-envoy-image#L4).
