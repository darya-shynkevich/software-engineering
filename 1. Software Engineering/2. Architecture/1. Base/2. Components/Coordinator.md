A coordinator node is the one where a request first comes in from the client. Client library keeps the information that which request should go to which node.

There can be two ways for a client to select a node:
- We route the request to a generic load balancer => the client isn’t linked to the code
- We use a partition-aware client library that routes requests directly to the appropriate coordinator nodes => the latency is lower due to the reduced number of hops because the client can directly go to a specific server

