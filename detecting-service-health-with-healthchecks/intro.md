In this scenario, you will learn how to add a health check that will check if a service within the cluster is available to receive traffic or not. With a health check enabled, if a service crashes then Envoy will stop sending traffic. Also you will learn how to configure Outlier Detection to eject hosts based on the responses from real requests.

You will learn how to:

* Add a HTTP health check / Outlier Detection

* Test load balancing options when a service is unavailable.

* Ensure how to continue to deliver successful requests when services are unavailable.