Why simply deleting cache would not solve the problem:

![[Pasted image 20231014202410.png]]

## Solutions:
#### A dirty patch
Whenever we delete the cache, make sure it is not outdated by another racing thread. So what if:
1. we delete it again after X minutes (archive eventually consistency).
2. X = the max out-of-sync minutes that can be tolerated.

![[Pasted image 20231014202424.png]]
#### Strong consistency

Sure then we have to sacrifice a bit of performance. using the distributed lock. you can either choose a database table or Redis (cluster) to do that.

1. When accessing the cache check the lock record, if no record then insert
2. Once done remove the lock record

This way could achieve strong consistency but the lock could be bottlenecked in the system throughput.

#### Versioning of cache item

When accessing a cache item:

1. Get the version of the cache item
2. Before updating(or deleting) it, if the version is the same then do update.

Wait, there is a chance that after checking, there is another thread that updated the value very quickly. True, there is such a case, but not always. and that’s why — versioning can not guarantee 100% strong consistency.

If you really need to make sure 100% and want a bit more throughput. maybe can combine it with the above locking, before updating, lock it then update then release the lock.

#### Multiple nodes

What if my cache is distributed? and when I delete one cache then need to notify other nodes the cache is invalid.

Remember how [[CPU level cache]] “tells” other “CPUs” that its cache is outdated? just use the same idea. At the CPU level, it uses bus snooping with MESI protocol, at the system level we can ***use a message queue(or subscribe to Mysql binlog) to do pub/sub.***

What if delete failed, Let’s say some nodes never ack the message. retry.