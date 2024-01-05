Engineers at Boeing invented the B-tree data structure in 1970 while on a quest to make searching large files faster. Despite their age, B-trees are still used widely in databases today and are the default index type in Postgres, MySQL, SQLite, Oracle, and SQL Server.



![Pasted image 20230605133326](../../../../_Attachments/Pasted%20image%2020230605133326.png)

***B+Trees*** allows us to build a tree structure where each intermediate node points to the highest node value of its respective leaf nodes. It ***gives us a clear path to find the index [Leaf Nodes](../1.%20Internals/Database%20Pages/Leaf%20Nodes.md) that will point to the necessary data***.

![Pasted image 20230605133604](../../../../_Attachments/Pasted%20image%2020230605133604.png)

##### B-Tree vs. B+Tree

*The main difference **B+ Trees** show off is that **intermediate nodes don't store any data on them**. Instead, all the data references are linked to the leaf nodes, which allows for better caching of the tree structure.

*Secondly, the **leaf nodes are linked, so if you need to do an index scan, you can do a single linear pass** rather than traversing the entire tree up and down and loading more index data from the disk.*

### Page Size

PostgreSQL and MySQL store table data in regular files on disk, while SQLite stores the entire database as a single file. Whether tables are broken up across multiple files or contained within a single file, data within the files is usually stored in units called "pages." In Postgres, the default page size is 8 kilobytes; in MySQL’s InnoDB storage engine, the default is 16 kilobytes.

> *In both cases (tables and indexes), the size of pages can matter because in theory, with larger page sizes the database can fit more rows in a single page, and thus it must fetch fewer pages from disk to serve a query. The actual performance impact will depend on your storage medium (SSD, HDD, etc.) and application, so you should profile when adjusting the page size.*

