When a client sends a request to a **load balancer**, the load balancer routes the request to one of the servers in its pool of servers using a load balancing algorithm:
1. **Round-robin**: The load balancer sends requests to servers in a rotation, sending the next request to the next server in the list.
2. **Least connections**: The load balancer sends requests to the server with the least number of active connections.
3. **Least response time**: The load balancer sends requests to the server that has the lowest response time for previous requests.
4. **Weighted round-robin**: The load balancer sends requests to servers in a rotation, but the rotation is based on the weight assigned to each server. Servers with a higher weight receive a higher proportion of the requests.

The load balancer can also use health checks to monitor the status of each server and route requests only to healthy servers. This helps to ensure that requests are not sent to servers that are down or experiencing issues.

Load balancers also use a **hash function** to redirect the user to server which they were connected before. It is important to assign the same user to same server as we have cache and other things related to that user saved on the server that can help us to server they better.

This looks good, but the problem is when servers are added and removed from the server pool.

Let’s suppose , due to increase in load, we added a new server and now we have 5 servers to serve the traffic. Now, the issue is when user A will make a request, it will be handled by completely different server because our servers increased and so does our hash function output, because before it was calculating the value considering 4 servers but now we have 5. so This is where the whole issue lies.

**Consistent hashing** is a hashing technique that allows nodes to be added or removed from a hash table (also called a distributed hash table or **DHT**) without significantly altering the mapping of keys to nodes.
	Consistent hashing is used in distributed systems design such as the [URL shortener](URL%20shortener%201) and [Pastebin](../Examples/Pastebin.md).
	***Consistent hashing minimises the number of keys to be remapped when the total number of nodes changes***

*In consistent hashing, the set of all possible keys is called the “hash space.” The hash space is typically represented as a circle, with the keys distributed evenly around the circle. This representation is called a “hash ring.”*

Hash ring with representation of hash space:
![Pasted image 20231217173438](../../../../_Attachments/Pasted%20image%2020231217173438.png)

Each node in the distributed system is also assigned a position on the hash ring, and is responsible for storing the keys that fall within a certain range of the hash ring. The range of keys that a node is responsible for is called its “zone.”

Using a hash function, we can map servers onto the ring:
![Pasted image 20231217173524](../../../../_Attachments/Pasted%20image%2020231217173524.png)

Now, we have our server mapped on to the ring, let’s understand how the requests will be handled by these servers.

We have three requests that mapped on to the hash space using the hash function
![Pasted image 20231217173556](../../../../_Attachments/Pasted%20image%2020231217173556.png)
Each request will be handled by the first server it encounter by moving to the clockwise direction. *In simple words, the first node with a position value greater than the position of the key stores the data object*
Based on the hash function, `r1` is places on the ring between **s4** and **s1**, so `r1` will be handled by the server **s1**. `r2` is places on the ring between **s2** and **s1**, so `r2` will be handled by the server **s2**. `r3` is places on the ring between **s4** and **s3**, so `r3` will be handled by the server **s4**.

## Adding a New Server

Let’s suppose we added a new server and based on the hash function , it is placed on the ring between s3 and s4.
![Pasted image 20231217173731](../../../../_Attachments/Pasted%20image%2020231217173731.png)
***Consistent hashing aid cloud computing by minimizing the movement of data when the total number of nodes changes due to dynamic load***

Average number of keys stored on a `node = k/N` where _k_ is the total number of keys (data objects) and _N_ is the number of nodes.

## Removing a Server

Removing a server from the ring will require only redistribution of few keys as well.
![Pasted image 20231217173827](../../../../_Attachments/Pasted%20image%2020231217173827.png)
Let’s suppose **s1** goes down, then the only request that need to redistributed is `r1` . Based on the clockwise approach, `r1` will be handled by the server **s2.**

## Issues With Consistent Hashing

### 1.  Uneven hash space

It is impossible to keep the size of each hash space on the ring same for all servers considering that new servers can be added and existing one can be removed.

It is possible that the size of the hash space on the ring assigned to each server is very small or fairly large.

### 2. Non uniform request distribution

There is a chance that nodes are not uniformly distributed on the consistent hash ring. The nodes that receive a huge amount of traffic become [Hotspot](Hotspot.md)s resulting in [cascading failure](https://en.wikipedia.org/wiki/Cascading_failure) of the nodes.

## Solution to the issues => Virtual nodes

Virtual nodes refers to real nodes. Each server we have on the ring has multiple virtual nodes. Let’s try to understand it with an example
Server 1 with 3 virtual nodes `vn 1_1`, `vn 1_2`, `vn 1_3`:
![Pasted image 20231217174154](../../../../_Attachments/Pasted%20image%2020231217174154.png)
In above diagram, we have 3 virtual nodes for server 1 that points to server 1, it means any request handled by the virtual nodes will be redirected to s1.

The technique of assigning multiple positions to a node is known as a **virtual node**. The virtual nodes improve the load balancing of the system and prevent hotspots. The number of positions for a node is decided by the heterogeneity of the node. In other words, ***the nodes with a higher capacity are assigned more positions on the hash ring***

Let’s take an example. Suppose we have three hash functions. ***For each node, we calculate three hashes and place them into the ring.*** For the request, we use ***only one hash function***. Wherever the request lands onto the ring, it’s processed by the next node found while moving in the clockwise direction. Each server has three positions, so the load of requests is more uniform. Moreover, ***if a node has more hardware capacity than others, we can add more virtual nodes by using additional hash functions. ***This way, it’ll have more positions in the ring and serve more requests.

(+) if a node fails or does routine maintenance, the workload is uniformly distributed over other nodes. For each newly accessible node, the other nodes receive nearly equal load when it comes back online or is added to the system.
(+) it’s up to each node to decide how many virtual nodes it’s responsible for, considering the heterogeneity of the physical infrastructure. For example, if a node has roughly double the computational capacity as compared to the others, it can take more load.

# Consistent hashing implementation

![Pasted image 20231217180410](../../../../_Attachments/Pasted%20image%2020231217180410.png)

The self-balancing [BST](BST) data structure is used to store the positions of the nodes on the hash ring. The BST offers logarithmic O(log n) time complexity for search, insert, and delete operations. The keys of the BST contain the positions of the nodes on the hash ring.

The BST data structure is stored on a centralised highly available service. As an alternative, the BST data structure is stored on each node, and the state information between the nodes is synchronised through the gossip protocol
### Insertion of a data object (key)

![Pasted image 20231217180522](../../../../_Attachments/Pasted%20image%2020231217180522.png)

In the diagram, suppose the hash of an arbitrary key ‘_xyz’_ yields the hash code output _5_. The successor BST node is _6_ and the data object with the key ‘_xyz_’ is stored on the node that is at position _6_. In general, the following operations are executed to insert a key (data object):
1. Hash the key of the data object
2. Search the BST in logarithmic time to find the BST node immediately greater than the hashed output
3. Store the data object in the successor node
### Insertion of a node
![Pasted image 20231217180652](../../../../_Attachments/Pasted%20image%2020231217180652.png)


The insertion of a new node results in the movement of data objects that fall within the range of the new node from the successor node. Each node might store an internal or an external BST to track the keys allocated in the node. The following operations are executed to insert a node on the hash ring:
1. Insert the hash of the node ID in BST in logarithmic time
2. Identify the keys that fall within the subrange of the new node from the successor node on BST
3. Move the keys to the new node
### Deletion of a node
![Pasted image 20231217180747](../../../../_Attachments/Pasted%20image%2020231217180747.png)

The deletion of a node results in the movement of data objects that fall within the range of the decommissioned node to the successor node. ***An additional external BST can be used to track the keys allocated in the node.*** The following operations are executed to delete a node on the hash ring:
1. Delete the hash of the decommissioned node ID in BST in logarithmic time
2. Identify the keys that fall within the range of the decommissioned node
3. Move the keys to the successor node

# What is the asymptotic complexity of consistent hashing?

![Pasted image 20231217180918](../../../../_Attachments/Pasted%20image%2020231217180918.png)

# How to handle concurrency in consistent hashing?

The BST that stores the positions of the nodes is a mutable data structure that must be synchronized when multiple nodes are added or removed at the same time on the hash ring. The [readers-writer lock](https://en.wikipedia.org/wiki/Readers%E2%80%93writer_lock) is used to synchronise [BST](BST) at the expense of a slight increase in latency.

# What hash functions are used in consistent hashing?

An optimal hash function for consistent hashing must be fast and produce uniform output. The cryptographic hash functions such as [MD5](https://en.wikipedia.org/wiki/MD5), and the secure hash algorithms [SHA-1](https://en.wikipedia.org/wiki/SHA-1) and [SHA-256](https://en.wikipedia.org/wiki/SHA-2) are not relatively fast. [MurmurHash](https://en.wikipedia.org/wiki/MurmurHash) is a relatively cheaper hash function. The non-cryptographic hash functions like [xxHash](https://github.com/cespare/xxhash), [MetroHash](https://github.com/dgryski/go-metro), or [SipHash1–3](https://github.com/dgryski/go-sip13) are other potential candidates

# What are the benefits of consistent hashing?

(+) horizontally scalable
(+) minimised data movement when the number of nodes changes
(+) quick replication and partitioning of data

The following are the advantages of virtual nodes:
	(+) load handled by a node is uniformly distributed across the remaining available nodes during downtime
	(+) the newly provisioned node accepts an equivalent amount of load from the available nodes
	(+) fair distribution of load among heterogenous nodes

# What are the drawbacks of consistent hashing?

(-) cascading failure due to hotspots
(-) non-uniform distribution of nodes and data
(-) oblivious to the heterogeneity in the performance of nodes

The following are the disadvantages of virtual nodes:
	(-) when a specific data object becomes extremely popular, consistent hashing will still send all the requests for the popular data object to the same subset of nodes resulting in a degradation of the service
	(-) capacity planning is trickier with virtual nodes
	(-) memory costs and operational complexity increase due to the maintenance of BST
	(-) replication of data objects is challenging due to the additional logic to identify the distinct physical nodes
	(-) downtime of a virtual node affects multiple nodes on the ring

# What are the consistent hashing examples?

1. The [Discord](../Examples/Discord.md) server (discord space or chat room) is hosted on a set of nodes. The client of the discord chat application identifies the set of nodes that hosts a specific discord server using consistent hashing
2. The distributed NoSQL data stores such as [Amazon DynamoDB](Amazon%20DynamoDB.md), Apache [Cassandra](../../../3.%20Database/OTLP/NoSQL/Types/Columnar%20Databases/Cassandra.md), and [Riak](Riak) use consistent hashing to dynamically partition the data set across the set of nodes. The data is partitioned for incremental scalability
3. The video storage and streaming service [Vimeo](Vimeo) uses consistent hashing for load balancing the traffic to stream videos
4. The video streaming service [Netflix](Netflix) uses consistent hashing to distribute the uploaded video content across the content delivery network (**CDN**)

# Consistent hashing optimization

1. Ben Appleton, Michael O’Reilly, [Multi-Probe Consistent Hashing](https://arxiv.org/abs/1505.00062) (2015), Google
2. Vahab Mirrokni, Mikkel Thorup, and Morteza Zadimoghaddam, [Consistent Hashing with Bounded Loads](https://arxiv.org/abs/1608.01350) (2017), Google

# References:

1. ~~[Consistent Hashing Explained](https://experiencestack.co/consistent-hashing-explained-89a446b19d3b)~~
2. ~~[Consistent Hashing](https://vishalrana9915.medium.com/consistent-hashing-36fa25892b4f)~~
3. ~~[Consistent Hashing | The Backend Engineering Show](!https://www.youtube.com/watch?v=p6wwj0ozifw&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=102)~~
4. ! [The Fundamental Knowledge of System Design (10) — Consistent Hashing](https://interviewnoodle.com/the-fundamental-knowledge-of-system-design-10-consistent-hashing-18fcefbfd749)
5. ! [Everything You Need to Know About Consistent Hashing](https://newsletter.systemdesign.one/p/what-is-consistent-hashing?utm_source=substack&publication_id=1511845&post_id=138987915&utm_medium=email&utm_content=share&utm_campaign=email-share&triggerShare=true&isFreemail=true&r=1vxw4z)
6. ! [Consistent hashing and rendezvous hashing explained.](https://www.francofernando.com/blog/distributed-systems/2021-12-24-distributed-hashing/)
7. Lindsey Kuper, [UC Santa Cruz CSE138 (Distributed Systems) Lecture 15: introduction to sharding; consistent hashing](https://www.youtube.com/watch?v=uNQGP0yupn0&list=PLNPUF5QyWU8PydLG2cIJrCvnn5I_exhYx&index=18) (2021)
8. David Karger, Eric Lehman, Tom Leighton, Rina Panigrahy, Matthew Levine, Daniel Lewin, [Consistent Hashing and Random Trees: Distributed Caching Protocols for Relieving Hot Spots on the World Wide Web](https://github.com/papers-we-love/papers-we-love/blob/master/distributed_systems/consistent-hashing-and-random-trees.pdf)
9. Tom White, [Consistent Hashing](http://tom-e-white.com/2007/11/consistent-hashing.html) (2007), tom-e-white.com
10. Srushtika Neelakantam, [Consistent hashing explained](https://ably.com/blog/implementing-efficient-consistent-hashing) (2018), ably.com
11. Giuseppe DeCandia, Deniz Hastorun, Madan Jampani, Gunavardhan Kakulapati, Avinash Lakshman, Alex Pilchin, Swaminathan Sivasubramanian, Peter Vosshall and Werner Vogels, [Dynamo: Amazon’s Highly Available Key-value Store](https://github.com/papers-we-love/papers-we-love/blob/master/datastores/dynamo-amazons-highly-available-key-value-store.pdf) (2007)
12. Damian Gryski, [Consistent Hashing: Algorithmic Tradeoffs](https://dgryski.medium.com/consistent-hashing-algorithmic-tradeoffs-ef6b8e2fcae8) (2018), medium.com
13. [MIT 6.854 Spring 2016 Lecture 3: Consistent Hashing and Random Trees](http://people.csail.mit.edu/moitra/854.html) (2016)
14. [Improving load balancing with a new consistent-hashing algorithm](https://medium.com/vimeo-engineering-blog/improving-load-balancing-with-a-new-consistent-hashing-algorithm-9f1bd75709ed) (2016), Vimeo Engineering Blog
15. Stanislav Vishnevskiy, [How discord scaled elixir to 5,000,000 concurrent users](https://discord.com/blog/how-discord-scaled-elixir-to-5-000-000-concurrent-users) (2017), discord.com
16. Mohit Vora, Andrew Berglund, Videsh Sadafal, David Pfitzner, and Ellen Livengood, [Distributing Content to Open Connect](https://netflixtechblog.com/distributing-content-to-open-connect-3e3e391d4dc9) (2017), netflixtechblog.com
17. [libketama — a consistent hashing algo for Memcache clients](https://www.last.fm/user/RJ/journal/2007/04/10/rz_libketama_-_a_consistent_hashing_algo_for_memcache_clients) (2007)
18. [What is Consistent Hashing and Where is it used?](https://www.youtube.com/watch?v=zaRkONvyGr8&list=PLMCXHnjXnTnvo6alSjVkgxV-VH6EPyvoX&index=5) (video)
19. [Start Here: Consistent Hashing](https://levelup.gitconnected.com/start-here-consistent-hashing-ee671b20a452)
20. [Consistent Hashing: Algorithmic Tradeoffs](https://dgryski.medium.com/consistent-hashing-algorithmic-tradeoffs-ef6b8e2fcae8)
21. [Amazon System Design Interview Question | Consistent Hashing](https://www.youtube.com/watch?v=5ARwdRPkYmc&list=PLOAph0xkZvSuqy8yq_0D6NEABhmSTRYrN&index=2) (video)
22. [Simple : Consistent Hashing](https://medium.com/omarelgabrys-blog/consistent-hashing-beyond-the-basics-525304a12ba)
23. [More On : Consistent Hashing](http://www.tom-e-white.com/2007/11/consistent-hashing.html)
24. [Distributed Consistent Hashing](https://medium.com/ably-realtime/how-to-implement-consistent-hashing-efficiently-fe038d59fff2)
25. [Bloom Filter : A Probabilistic Data Structure](https://medium.com/system-design-blog/bloom-filter-a-probabilistic-data-structure-12e4e5cf0638)
26. [Consistent Hashing](http://www.tom-e-white.com/2007/11/consistent-hashing.html)
27. [Consistent Hashing](https://medium.com/@meenak1996/why-we-need-of-consistent-hashing-42b87c65596b)
28. [Consistent Hashing: Algorithmic Tradeoffs](https://medium.com/@dgryski/consistent-hashing-algorithmic-tradeoffs-ef6b8e2fcae8)
29. [Don’t be tricked by the Hashing Trick](https://booking.ai/dont-be-tricked-by-the-hashing-trick-192a6aae3087)
30. [Uniform Consistent Hashing at Netflix](https://medium.com/netflix-techblog/distributing-content-to-open-connect-3e3e391d4dc9)
