1. **Horizontally scalable:** Read/ write throughput, as well as storage capacity, can be increased *almost* linearly by adding additional nodes to the Elasticsearch cluster. 
	   -> Adding nodes to a cluster is relatively effortless and can be done without any downtime. 
	   -> The cluster can automatically redistribute shards evenly across nodes (subject to shard allocation filtering rules) as the number of nodes available changes to optimize performance and improve resource utilization across nodes.
2. **Highly available and resilient:** By default, Elasticsearch will allocate one replica for every primary shard on different nodes in the cluster, making Elasticsearch a highly available system where requests can still be completed when one or more nodes experience failures.
	   -> If a node holding a primary shard is lost, a replica shard will be selected and promoted to become a primary shard, and a replica shard will be allocated to another node in the cluster.
	   -> If a node holding a replica shard is lost, the replica shard will simply be allocated to another node in the cluster.
3. **Recoverable from disasters:** Elasticsearch allows us to persistently store or snapshot data, making it a recoverable system in the event of a disaster.
	   -> Snapshots can be configured to write data to a traditional filesystem or an object store such as AWS S3.
	   -> Snapshots are a point-in-time copy of the data and must be taken at regular intervals, depending on your [[Recovery Point Objective]] (RPO), for an effective disaster recovery plan to be created.
4. **Cross-cluster operations:** Elasticsearch can search for and replicate data across remote clusters to enable more sophisticated architectural patterns.
	   1. Cross-Cluster Search (CCS) is a feature that allows you to search data that resides on an external or remote Elasticsearch cluster.
	      ![](Pasted%20image%2020241225163818.png)
	   2. Cross-cluster replication (CCR) allows an index to be replicated in a local cluster to a remote cluster. CCR implements a leader/follower model, where all the changes that have been made to a leader index are replicated on the follower index. This feature allows for fast searching on the same dataset in different geographical regions by replicating data closer to where it will be consumed. CCR is also sometimes used for cross-region redundancy requirements:
	      ![](Pasted%20image%2020241225163916.png)
	CCS and CCR enable more complex use cases where multiple regional clusters can be used to independently store and search for data, while also allowing unified search and geographical redundancy.  
5. **Security:** Elasticsearch offers security features to help authenticate and authorize user requests, as well as encrypting network communications to and within the cluster.