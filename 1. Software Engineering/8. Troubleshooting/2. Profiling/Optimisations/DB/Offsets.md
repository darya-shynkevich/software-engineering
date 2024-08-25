Offset by design means fetch and drop the first x number of rows, so in the case `page=10&count=10` case the database will fetch the first 110 rows and physically drop the first 100 leaving the limit of 10 which the user will get. As the offset increase, the database is doing more work which makes this operation extremely expensive.

Furthermore, the problem with offset is you might accidentally read duplicate records. consider the user now want to read page 11 and meanwhile someone inserted a new row in the table, row 111 will be read twice

**Problems:**
1. Bad performance (can especially hit SQL Server because of [Transaction Range Locks](../../../../3.%20Database/OTLP/SQL/3.%20Transactions/Transaction%20Locks/Transaction%20Range%20Locks.md))
2. Duplicates on read

![](../../../../../_Attachments/Pasted%20image%2020240107180552.png)
![](../../../../../_Attachments/Pasted%20image%2020240107180635.png)

# How to fix

It is possible to use indexed filed (like `id`) and `limit`

![](../../../../../_Attachments/Pasted%20image%2020240107180753.png)

# References:

1. ~~[don’t use “offset” in your SQL](https://www.youtube.com/watch?v=WDJRRNCGIRs&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=57) (video)~~