
*==As a general rule the longer a transaction is open the more contention and potentially failure can occur.==*

Two-phase commit:
- Leader writes a durable transaction record indicating a cross-shard transaction.
- Participants write a permanent record of their willingness to commit and notify the leader.
- The leader commits the transaction by updating the durable transaction record after receiving all responses. (It can abort the transaction if no one responds.)
- Participants can show the new state after the leader announces the commit decision. (They delete the staged state if the leader aborts the transaction.)

    *Read-and-write amplification in the protocol path is a major issue. Write amplification occurs because you must write a transaction record and durably stage a commit, which requires at least one write per participant. Lock contention and application instability can result from excessive writes. ==The database must additionally filter every read to ensure that it does not see any state that is dependent on a pending cross-shard transaction, which affects all reads in the system, even non-transactional ones.

# References:

1. ! [How to Solve the Dual Write Problem: 4 Essential Patterns Every Developer Should Know](https://levelup.gitconnected.com/exploring-the-dual-write-problem-and-solutions-d0ebea224926)
2. [Transactions in a Microservice World](https://wso2.com/whitepapers/transactions-in-a-microservice-world/)
3. [Transactions across data centers](http://snarfed.org/transactions_across_datacenters_io.html)
4. [Distributed transaction patterns for microservices compared](https://developers.redhat.com/articles/2021/09/21/distributed-transaction-patterns-microservices-compared#)
5. [Cross Shard Transactions at 10 Million RPS at Dropbox](https://blogs.dropbox.com/tech/2018/11/cross-shard-transactions-at-10-million-requests-per-second/)
6. [Handling Distributed Transactions in Microservices](https://blog.bitsrc.io/distributed-transactions-in-microservices-d07aba281f90)
7. [What is a Distributed Transaction in Microservices?](https://www.youtube.com/watch?v=H6F4BorD49g&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=32) (video)
8. [Exploring Solutions for Distributed Transactions](https://medium.com/thedevproject/exploring-solutions-for-distributed-transactions-1-15853eebc114)
9. [Distributed Transaction Management in Microservices](https://medium.com/javarevisited/distributed-transaction-management-in-microservices-part-1-bb7dc1fbee9f)
10. [Life Beyond Distributed Transactions](https://docs.microsoft.com/en-us/archive/blogs/pathelland/link-to-quotlife-beyond-distributed-transactions-an-apostates-opinion) - Helland
11. [Distributed transactions](https://www.youtube.com/playlist?list=PLkQkbY7JNJuBz758qIPSppFZm77aP6LYJ) (videos)
12. [Google I/O 2009 - Transactions Across Datacenters](https://www.youtube.com/watch?v=srOgpXECblk) (video)
13. [The end of a myth: Distributed transactions can scale](https://muratbuffalo.blogspot.com/2023/04/the-end-of-myth-distributed.html?m=1)
14. [Распределенные транзакции и путешествия во времени](https://www.youtube.com/watch?v=DFLEkWi-vRc) (video)
15. [Распределенные транзакции в YDB](https://www.youtube.com/watch?v=8AR1u5OZIm8) (video)
16. [Cross shard transactions at 10 million requests per second](https://dropbox.tech/infrastructure/cross-shard-transactions-at-10-million-requests-per-second?utm_source=substack&utm_medium=email)
17. [Reservation Pattern: Can it replace distributed transactions?](https://blog.devgenius.io/reservation-pattern-can-it-replace-distributed-transactions-de2481240e20)
18. [I asked this system design question to 3 guys during a developer interview and none of them gave the answer](https://iorilan.medium.com/i-asked-this-system-design-question-to-3-guys-during-a-developer-interview-and-none-of-them-gave-9c23abe45687)

**2PC:**
	1. [Avoiding Two-Phase Commit](https://web.archive.org/web/20180821165044/http://www.addsimplicity.com/adding_simplicity_an_engi/2006/12/avoiding_two_ph.html) - Two phase commit avoidance approaches
	2. [2PC or not 2PC, Wherefore Art Thou XA?](https://web.archive.org/web/20180821164931/http://www.addsimplicity.com/adding_simplicity_an_engi/2006/12/2pc_or_not_2pc_.html) - Two phase commit isn't a silver bullet

**[Presto the Distributed SQL Query Engine](!https://research.fb.com/wp-content/uploads/2019/03/Presto-SQL-on-Everything.pdf?):**
	1. [Presto at Pinterest](https://medium.com/@Pinterest_Engineering/presto-at-pinterest-a8bda7515e52)
	2. [Presto Infrastructure at Lyft](https://eng.lyft.com/presto-infrastructure-at-lyft-b10adb9db01)
	3. [Presto at Grab](https://engineering.grab.com/scaling-like-a-boss-with-presto)
	4. [Engineering Data Analytics with Presto and Apache Parquet at Uber](https://eng.uber.com/presto/)
	5. [Data Wrangling at Slack](https://slack.engineering/data-wrangling-at-slack-f2e0ff633b69)
	6. [Presto in Big Data Platform on AWS at Netflix](https://medium.com/netflix-techblog/using-presto-in-our-big-data-platform-on-aws-938035909fd4)
	7. [Presto Auto Scaling at Eventbrite](https://www.eventbrite.com/engineering/big-data-workloads-presto-auto-scaling/)
	8. [Speed Up Presto with Alluxio Local Cache at Uber](https://www.uber.com/en-MY/blog/speed-up-presto-with-alluxio-local-cache/)