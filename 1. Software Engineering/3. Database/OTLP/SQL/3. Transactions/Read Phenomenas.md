# 1. Dirty Reads

*A dirty read occurs when you perform a read, and another transaction updates the same row but doesn't commit the work, you perform another read, and you can access the uncommitted (dirty) value, which isn't a durable state change and is inconsistent with the stat
e of the database.*


![Pasted image 20230605134357](../../../../../_Attachments/Pasted%20image%2020230605134357.png)

# 2. **Phantom Reads**

*Phantom reads are another committed read phenomena, which occurs when you are most commonly dealing with aggregates. For example, you ask for the number of customers in a specific transaction. Between the two subsequent reads, another customer signs up or deletes their account (committed), which results in you getting two different values if your database doesn't support range locks for these transactions.*

![Pasted image 20231210223824](../../../../../_Attachments/Pasted%20image%2020231210223824.png)
Example:
1. T1: Select all `QNT * PRICE` (sum = 130)
2. T2: `INSERT` `Product 3`
3. T1: `SUM(QNT * PRICE)` (sum = 140)

# 3.  Non-Repeatable Reads

*Non-repeatable reads occur if you cannot get a consistent view of the data between two subsequent reads during your transaction. In specific modes, concurrent database modification is possible, and there can be scenarios where the value you just read can be modified, resulting in a non-repeatable read.*

![Pasted image 20231210223424](../../../../../_Attachments/Pasted%20image%2020231210223424.png)
Example:
1. T1: Select all `QNT * PRICE` (sum = 130)
2. T2: Update `Product 1` `QNT = 15`
3. T1: `SUM(QNT * PRICE)` (sum = 155)

# References:

1. [A beginner’s guide to Non-Repeatable Read anomaly](https://vladmihalcea.com/non-repeatable-read/)
2. [Phantom Reads in Postgres Explained Compared to Other DBMS](!https://www.youtube.com/watch?v=MEAD5JNc_Dw&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=66)