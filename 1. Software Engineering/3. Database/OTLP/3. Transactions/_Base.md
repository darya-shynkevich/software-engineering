Транзакция - действие / серия действий которые осущетствляют доступ или изменение содержимого БД
1. a collection of queries
2. one unit of work

Транзакции переводят БД из одного согласованного состояния в другое.

In computer science, **ACID** (atomicity, consistency, isolation, durability) is a set of properties of database transactions intended to guarantee data validity despite errors, power failures, and other mishaps.

- A guarantee of ==**Atomicity**== prevents updates to the database from occurring only partially, which can cause more significant problems than rejecting the whole series outright. 
  All or nothing: all queries must succeed. If one fails, the rest should rollback.
- ==**[Isolation](Isolation.md)**== determines how a particular action is shown to other concurrent system users.
- ==**Consistency**== guarantees that transaction can move database from one valid state to the next. This ensures these all adhere to all defined database rules. Also preventing corruption by illegal transaction. 
		1. Consistency in Data
			1. Defined by the user
			2. Referential integrity (FK)
			3. Atomicity
			4. [Isolation](Isolation.md)
		2. Consistency in Reads
			1. If a transaction committed a change will a new transaction immediately see the change?
			2. Relational and NoSQL DBs suffer from this. Because even relational DBs are not consistent in reads (example: adding more replicas)
			3. [Eventual Consistency](Eventual%20Consistency)
- ==**Durability**== is the property that guarantees that transactions that have been committed will survive permanently.
	- Redis, Memcached are not durable DBs
# [Isolation](Isolation.md) Levels

# [_Base](Transaction%20Locks/_Base.md)

# [Transaction concurrency](Transaction%20concurrency.md)

# References:

1. [Transaction Processing: Concepts and Techniques](https://www.amazon.com/Transaction-Processing-Concepts-Techniques-Management/dp/1558601902) (book)
2. [На пути к правильным SQL транзакциям (Часть 1)](https://habr.com/ru/companies/infopulse/articles/261097/)
3. ~~[Relational Database ACID Transactions (Explained by Example)](https://www.youtube.com/watch?v=pomxJOFVcQs&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2) (video)~~
4. ~~[Relational Database Atomicity Explained By Example](https://www.youtube.com/watch?v=6vqzOjfZDco&list=PLQnljOFTspQXOkIpdwjsMlVqkIffdqZ2K&index=32) (video)~~
5. ~~[Can you get Eventual Consistency in Relational Databases?](https://www.youtube.com/watch?v=ryD9IA9i-c8&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=6) (video)~~
6. [Drop ACID And Think About Data](http://highscalability.com/blog/2009/5/5/drop-acid-and-think-about-data.html)
7. [Базы данных. MySQL. Транзакции](https://www.youtube.com/watch?v=qb6l4B57Qmw) (video)
8. [ACID: Изоляция. (Владимир Кузнецов)](https://www.youtube.com/watch?v=TMGAN-2iy1s&list=PLmqFxxywkatR3Psg4pz0Br0uDHzjR9Sne&index=5) (video)
9. [Базы данных. Транзакции. Триггеры и хранимые процедуры](https://www.youtube.com/watch?v=XkS3937Xn8M)
10. [Анатомия конкурентного доступа к данным в PostgreSQL](https://www.youtube.com/watch?v=J_ZCo6RBNj4&list=PLH-XmS0lSi_xHPmiMdgH9uSW9vBK1yP1A&index=6) (video)
11. [Работаем с транзакциями в БД правильно](https://www.youtube.com/watch?v=L28Y_Lg56c8&list=PLH-XmS0lSi_yVB6gNPkgA_ziD70q_8JFC) (video)
12. [Транзакции](https://www.youtube.com/watch?v=MF_kk3qV010) (video)
13. [Transactions: myths, surprises and opportunities](https://www.youtube.com/watch?v=5ZjhNTM8XU8) (video)