
![Pasted image 20230606192017](../../../../../../_Attachments/Pasted%20image%2020230606192017.png)

### No persistence

This is the fastest way to run Redis and has no durability guarantees.

### RDB Files

**RDB** (Redis Database): The RDB persistence *performs point-in-time snapshots of your dataset at specified intervals*.

The main downside to this mechanism is that *data between snapshots will be lost*. In addition, this storage mechanism also relies on forking the main process, and in a larger dataset, this may lead to a momentary delay in serving requests. *That being said, RDB files are much faster being loaded in memory than AOF.*

### AOF

**AOF** (Append Only File): *The AOF persistence logs every write operation the server receives that will be played again at server startup, reconstructing the original dataset.*

This way of ensuring persistence is much more durable than RDB snapshots since it is an append-only file. As operations happen, we buffer them to the log, but they aren't persisted yet. *This log consists of the actual commands we ran in order for replay when needed.*

Then when possible, we flush it to disk with [fsync](../../../../../6.%20Linux/fsync.md) (when this runs is configurable), it will be persisted. *The downside is that the format isn't compact and uses more disk than RDB files.*

### Why not both?

**RDB + AOF**: It is possible to combine AOF and RDB in the same Redis instance. If durability in exchange for some speed is a tradeoff, you are willing to make it. I think this is an acceptable way to set up Redis. *In the case of a restart, remember that if both are enabled, Redis will use AOF to reconstruct the data since it's the most complete.*

## Forking

*How redis uses forking for point in time snapshots*

![Pasted image 20230606192704](../../../../../../_Attachments/Pasted%20image%2020230606192704.png)

*[Forking](https://en.wikipedia.org/wiki/Fork_(system_call)?ref=architecturenotes.co) is a way for operating systems to create new processes by creating copies of themselves.* With this, you get a new process ID and a few other bits of information and handles, so the newly forked process (child) can talk to the original process parent.

When you fork a process, the parent and child share memory, and in that child process Redis begins the snapshotting (Redis) process. This is made possible by a memory sharing technique called *[Copy-on-write](../../../../../6.%20Linux/Copy-on-write.md)* — *which passes references to the memory at the time the fork was created. If no changes occur while the child process is persisting to disk, no new allocations are made.*

In the case where there are changes, the kernel keeps track of references to each page, and if there are more than one to specific page the changes are written to new pages. The child process is fully unaware of the change and has consistent memory snapshot. Therefore only fraction of the memory is used and we are able to achieve a point in time snapshot of potentially gigabytes of memory extremely quickly and efficiently!

*It's like taking a picture of something that hasn't moved - you don't need to take a whole new picture, just the parts that have changed.*

## References:

1. ~~[Redis Explained](https://architecturenotes.co/p/redis)~~