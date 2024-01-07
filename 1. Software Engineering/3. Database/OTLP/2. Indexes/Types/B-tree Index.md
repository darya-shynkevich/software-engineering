Used in Postgres, MySQL, SQLite, Oracle, and SQL Server.



![Pasted image 20230605133326](../../../../../_Attachments/Pasted%20image%2020230605133326.png)

***B+Trees*** allows us to build a tree structure where each intermediate node points to the highest node value of its respective leaf nodes. It ***gives us a clear path to find the index [leaf node](../../1.%20Internals/Database%20Pages/Leaf%20Nodes.md) that will point to the necessary data***.

![Pasted image 20230605133604](../../../../../_Attachments/Pasted%20image%2020230605133604.png)

##### B-Tree vs. B+Tree

The main difference **B+ Trees** show off is that 
1. **intermediate nodes don't store any data on them**. Instead, all the data references are linked to the leaf nodes, which allows for better performance (more indexes on the page => faster search) and caching of the tree structure.
2. the **leaf nodes are linked, so if you need to do an index scan, you can do a single linear pass** rather than traversing the entire tree up and down and loading more index data from the disk.

MongoDB uses pure B-tree => that's why [Discord migrate from it to another DB](https://discord.com/blog/how-discord-stores-billions-of-messages): *"The messages were stored in a MongoDB collection with a single compound index on channel_id and created_at. Around November 2015, we reached 100 million stored messages and at this time we started to see the expected issues appearing: the data and the index could no longer fit in RAM and latencies started to become unpredictable. It was time to migrate to a database more suited to the task."* 

### Page Size

PostgreSQL and MySQL store table data in regular files on disk, while SQLite stores the entire database as a single file. Whether tables are broken up across multiple files or contained within a single file, data within the files is usually stored in units called "pages." In Postgres, the default page size is 8 kilobytes; in MySQL’s InnoDB storage engine, the default is 16 kilobytes.

> *In both cases (tables and indexes), the size of pages can matter because in theory, with larger page sizes the database can fit more rows in a single page, and thus it must fetch fewer pages from disk to serve a query. The actual performance impact will depend on your storage medium (SSD, HDD, etc.) and application, so you should profile when adjusting the page size.*

# References:

1. ~~[B-tree vs B+ tree in Database Systems](https://www.youtube.com/watch?v=UzHl2VzyZS4&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=80) (videos)~~