Now, we should generate traffic performing requests to the envoy proxy using this command:

`while true; do curl localhost; sleep .5; done`{{execute T2}}

Noticed that one of the endpoints process more request than the other according to you configuration. 
