**Graph databases** use the graph data structure to store data, where nodes represent entities, and edges show relationships between entities. The organization of nodes based on relationships leads to interesting patterns between the nodes. This database allows us to store the data once and then interpret it differently based on relationships. Popular graph databases include [[Neo4J]], [[OrientDB]], and [[InfiniteGraph]]. Graph data is kept in store files for persistent storage. Each of the files contains data for a specific part of the graph, such as nodes, links, properties, and so on.

In the following figure, some data is stored using a graph data structure in nodes connected to each other via edges representing relationships between nodes. Each node has some properties, like `Name`, `ID`, and `Age`. The node having `ID: 2` has the `Name` of `James` and `Age` of `29` years.

![](../../../../../../_Attachments/Pasted%20image%2020240119185852.png)

**Use case**: Graph databases can be used in social applications and provide interesting facts and figures among different kinds of users and their activities. The focus of graph databases is to store data and pave the way to drive analyses and decisions based on relationships between entities. The nature of graph databases makes them suitable for various applications, such as data regulation and privacy, machine learning research, financial services-based applications, and many more.

# References:

1. [**Graph Databases**](https://www.eecs.harvard.edu/margo/papers/systor13-bench/)
2. **[On Graph Databases | The Backend Engineering Show](https://www.youtube.com/watch?v=tvuZ-wqSTi0&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=87)** (video)
3. [on graph databases](https://www.youtube.com/watch?v=6DfjcUMNgYE&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=86) (video)
4. [FlockDB: Distributed Graph Database at Twitter](https://blog.twitter.com/engineering/en_us/a/2010/introducing-flockdb.html)
5. [TAO: Distributed Data Store for the Social Graph at Facebook](https://www.cs.cmu.edu/~pavlo/courses/fall2013/static/papers/11730-atc13-bronson.pdf)
6. [Akutan: Distributed Knowledge Graph Store at eBay](https://tech.ebayinc.com/engineering/akutan-a-distributed-knowledge-graph-store/)