When a filter needs to acquire a connection to a host in an upstream cluster, the cluster manager uses a load balancing policy to determine which host is selected. The load balancing policies are pluggable and are specified on a per upstream cluster basis in the configuration

## Supported load balancers

This are some of the most common used load balancers:

### Weighted round robin
This is a simple policy in which each available upstream host is selected in round robin order. If weights are assigned to endpoints in a locality, then a weighted round robin schedule is used, where higher weighted endpoints will appear more often in the rotation to achieve the effective weighting.

### Weighted least request
The least request load balancer uses an O(1) algorithm which selects two random healthy hosts and picks the host which has fewer active requests

### Ring hash
The ring/modulo hash load balancer implements consistent hashing to upstream hosts. The algorithm is based on mapping all hosts onto a circle such that the addition or removal of a host from the host set changes only affect 1/N requests.

### Random
The random load balancer selects a random healthy host. The random load balancer generally performs better than round robin if no health checking policy is configured.

### Original destination
This is a special purpose load balancer that can only be used with an original destination cluster. Upstream host is selected based on the downstream connection metadata, i.e., connections are opened to the same address as the destination address of the incoming connection was before the connection was redirected to Envoy.

Se more about the policies in [Envoy documentation](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/load_balancing/load_balancers).
