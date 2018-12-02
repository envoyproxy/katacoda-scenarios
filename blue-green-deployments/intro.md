Having the flexability to release software changes to a private and small subset of users to verify and test changes can have a huge positive impact on the deployment process. 

This happens has a number of names and approaches, such as Blue Green Deployments and Canary Releases. The aim of is to enable changes to be rolled out more frequently, with higher confident.

In this scenario you will learn how to perform blue green and canary releases with Envoy Proxy. The scenario will explain:

* Access private deployments via HTTP Headers

* Shift 20% of traffic to a new release to test against production traffic

* Roll out 100% of traffic to a new release