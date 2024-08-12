> Maintain a list of counters, one per cluster node, to detect concurrent updates

**Problem:** If multiple servers allow the same key to be updated, it's important to detect when the values are concurrently updated across a set of replicas.

**Solution:**

Each key value is associated with a version vector that maintains a number for each cluster node.

In essence, a version vector is a set of counters, one for each node. A version vector for three nodes (blue, green, black) would look something like `[blue: 43, green: 54, black: 12]`. Each time a node has an internal update, it updates its own counter, so an update in the green node would change the vector to `[blue: 43, green: 55, black: 12]`. Whenever two nodes communicate, they synchronise their vector stamps, allowing them to detect any simultaneous updates.

# References:

1. ~~[Version Vector by Martin Fowler](https://martinfowler.com/articles/patterns-of-distributed-systems/version-vector.html)~~