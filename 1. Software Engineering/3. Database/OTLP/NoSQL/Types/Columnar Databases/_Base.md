**Columnar databases** store data in columns instead of rows. They enable access to all entries in the database column quickly and efficiently. Popular columnar databases include [[Cassandra]], [[HBase]], [[Hypertable]], and Amazon [[Redshift]].

**Use case**: ***Columnar databases are efficient for a large number of aggregation and data analytics queries.*** ***It drastically reduces the disk I/O requirements and the amount of data required to load from the disk.*** For example, in applications related to financial institutions, there’s a need to sum the financial transaction over a period of time. Columnar databases make this operation quicker by just reading the column for the amount of money, ignoring other attributes of customers.

The following figure shows an example of a columnar database, where data is stored in a column-oriented format. This is unlike relational databases, which store data in a row-oriented fashion:

![](../../../../../../_Attachments/Pasted%20image%2020240119190110.png)

# References:

1. [Columnar Databases](https://aws.amazon.com/nosql/columnar/)
2. [Column vs Row Oriented Databases](https://www.youtube.com/watch?v=Vw1fCeD06YI&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=5) [Explained](https://www.youtube.com/watch?v=Vw1fCeD06YI&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=5) (video)