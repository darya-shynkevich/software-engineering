
**Optimistic locking** is the most common method to solve [Lost Update Phenomena](Lost%20Update%20Phenomena.md): allows any update of a record to happen only when the value of that record has not changed after its last read. If not true => fail the transaction.
	(-) more burden on the client side

**Pessimistic locking** is also a way to prevent lost update problems in DBMS. In this approach, ***the DB objects that are going to be updated are explicitly locked using the ‘PESSIMISTICWRITE’ mode***. After locking the object read-modify-write operations are performed on the objects and then the object is released. During these operations, if another transaction tries to read the same object it has to wait until the read-modify-write cycle of the first transaction is completed.
	Example: taking your umbrella every time you go out.

Disadvantages:
	(-) expensive + they are need to be managed

# How DBs manage locks

Optimistic:
1. **MongoDB:**

Pessimistic (all RDMS)
1. **SQL Server**: in memory  => "lock escalation" = table lock ([Transaction Range Locks](Transaction%20Range%20Locks.md))
2. **Postgres**: in disk (you will write to disk anyway)
3. **Oracle**

# References:

1. ~~[Should you go with an Optimistic or Pessimistic Concurrency Control Database?](!https://www.youtube.com/watch?v=H_zJ81I_D5E&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=81)~~
2. [Pessimistic concurrency control vs Optimistic concurrency control in Database Systems Explained](https://www.youtube.com/watch?v=I8IlO0hCSgY&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=18) (video