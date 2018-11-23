tracing:
  http:
    name: envoy.dynamic.ot
    config:
      library: /usr/local/lib/libjaegertracing_plugin.so
      config:
        service_name: service1
        sampler:
          type: const
          param: 1
        reporter:
          localAgentHostPort: jaeger:6831
        headers:
          jaegerDebugHeader: jaeger-debug-id
          jaegerBaggageHeader: jaeger-baggage
          traceBaggageHeaderPrefix: uberctx-
        baggage_restrictions:
          denyBaggageOnInitializationFailure: false
          hostPort: ""