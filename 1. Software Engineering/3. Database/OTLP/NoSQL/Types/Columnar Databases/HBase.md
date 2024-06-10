Pinterest hosted one of the largest production deployments of HBase in the 
world. 

(+) durable 
(+) scalable
(+) generally performant
(+) relatively simple NoSQL interface

(-) **high** **maintenance cost** 
	tech debt
	reliability risks
	hard to find HBase domain experts
	barriers to entry are very high for new engineers)
(-) **missing functionalities** 
	distributed transactions (Sparrow on top of [Apache Phoenix Omid](https://omid.incubator.apache.org/))
	global secondary index ([Ixia](https://medium.com/pinterest-engineering/building-scalable-near-real-time-indexing-on-hbase-7b5eeb411888), and [Manas realtime](https://medium.com/pinterest-engineering/manas-realtime-enabling-changes-to-be-searchable-in-a-blink-of-an-eye-36acc3506843))
	rich query capabilities
	high system complexity (because of the need to provide these advanced features for customers)
(-) **high infra cost**: *[production HBase clusters typically used a primary-standby setup with six data replicas for fast disaster recovery, which, however, came at an extremely high infra cost at our scale](https://medium.com/pinterest-engineering/hbase-deprecation-at-pinterest-8a99e6c8e6b7)*. With careful replication and placement mechanisms, TiDB, Rockstore, or MySQL may use three replicas without sacrificing much on availability SLA
(-) **waning industry usage and community support** 
	shrinking talent pool 
	higher barrier to entry 
	lower incentive for new engineers to become a subject matter expert of HBase


Pinterest decided to migrate online analytics workloads to [Druid](https://medium.com/pinterest-engineering/pinterests-analytics-as-a-platform-on-druid-part-1-of-3-9043776b7b76)/[StarRocks](https://www.starrocks.io/), time series data to [Goku](https://medium.com/pinterest-engineering/goku-building-a-scalable-and-high-performant-time-series-database-system-a8ff5758a181), an in-house time-series datastore, and key value use cases to [KVStore](https://medium.com/@Pinterest_Engineering/3-innovations-while-unifying-pinterests-key-value-storage-8cdcdf8cf6aa)

=> To accommodate the remaining HBase use cases, we needed a new technology that offers great scalability like a NoSQL database while supporting powerful query capabilities and ACID semantics like a traditional RDBMS. We ended up choosing [TiDB](https://github.com/pingcap/tidb), a distributed NewSQL database that satisfied most of our requirements.

# References:

1. [HBase](https://hbase.apache.org/)
2. [HBase at Salesforce](https://engineering.salesforce.com/investing-in-big-data-apache-hbase-b9d98661a66b)
3. [HBase in Facebook Messages](https://www.facebook.com/notes/facebook-engineering/the-underlying-technology-of-messages/454991608919/)
4. [HBase in Imgur Notification](https://blog.imgur.com/2015/09/15/tech-tuesday-imgur-notifications-from-mysql-to-hbase/)
5. [Improving HBase Backup Efficiency at Pinterest](https://medium.com/@Pinterest_Engineering/improving-hbase-backup-efficiency-at-pinterest-86159da4b954)
6. [HBase at Xiaomi](https://www.slideshare.net/HBaseCon/hbase-practice-at-xiaomi)
7. ~~[HBase Deprecation at Pinterest](https://medium.com/pinterest-engineering/hbase-deprecation-at-pinterest-8a99e6c8e6b7)~~