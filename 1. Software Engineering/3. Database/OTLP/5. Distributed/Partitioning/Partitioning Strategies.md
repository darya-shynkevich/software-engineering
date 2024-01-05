![Pasted image 20230826021729](../../../../../_Attachments/Pasted%20image%2020230826021729.png)

*==Data partitioning divides data within tables into smaller units called partitions.==*

There are two main strategies for data partitioning:

#### Horizontal Partitioning (Sharding)

- Horizontal partitioning divides rows of a table into smaller tables, often called shards, and distributes them across different servers or database instances. Each shard contains the same set of columns but a subset of rows from the original table. Sharding is primarily used to distribute the load of a large database across multiple servers. It’s particularly useful for handling large amounts of data in high-traffic applications.
- Sharding can be done based on different criteria, such as range-based or hash-based sharding. Range-based sharding divides rows based on the values of a specific column (e.g., partitioning orders by order date ranges). Hash-based sharding uses a hash function on a column’s value to determine which shard the row belongs to.
- However, it can bring more complex querying across shards, especially for operations involving multiple shards.

#### Vertical Partitioning

- Vertical partitioning splits columns of a table into separate tables. Each new table retains the same rows as the original table but contains a subset of columns. The goal of vertical partitioning is to optimize query performance, especially for queries that access a limited number of columns. ***It can also be used to isolate sensitive data from less sensitive data for security purposes.***
- Vertical partitioning can be applied to both normalisation and denormalisation processes. When normalised, related data is split into different tables to minimise redundancy and improve data integrity. When denormalised, non-related columns are split into separate tables to optimise performance.
- However, it brings increased complexity in querying when retrieving data from multiple vertically partitioned tables.