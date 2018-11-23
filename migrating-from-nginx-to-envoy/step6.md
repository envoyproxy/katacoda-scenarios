The final configuration required is the logging. Instead of piping the error logs to disk, Envoy Proxy follows a cloud native approach where all the logs are outputted to stdout and stderr.

TODO: FACT CECHK?!?

Envoy Proxy includes additional functionality to include tracing of requests. To learn more, read the [tracing documentation]().