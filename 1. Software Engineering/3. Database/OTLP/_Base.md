*are typically user-facing, which means that they may see a huge volume of requests. In order to handle the load, applications usually only touch a small number of records in each query. Disk seek time is often the bottleneck here.*

Some insights about [_ Base](1.%20Internals/_%20Base.md) (HOW DB works under the hood)

- The[ log-structured school](%20log-structured%20school), which only permits appending to files and deleting obsolete files, but never updates a file that has been written. Bitcask, SSTables, LSM-trees, LevelDB, Cassandra, HBase, Lucene, and others belong to this group.
    
- The [update-in-place school](update-in-place%20school), which treats the disk as a set of fixed-size pages that can be overwritten. B-trees are the biggest example of this philosophy, being used in all major relational databases and also many nonrelational ones.

![Pasted image 20230605130337](../../../_Attachments/Pasted%20image%2020230605130337.png)

*[_Base](2.%20Indexes/_Base.md) are a data structure that helps decrease the look-up time of requested data. Indexes achieve this with the additional costs of storage, memory, and keeping it up to date (slower writes), which allows us to skip the tedious task of checking every table row.*

*A **[_Base](3.%20Transactions/_Base.md) is a unit of work you want to treat as a single unit**. Therefore, it has to either happen in full or not at all. I would argue most systems don't need to manage transactions manually, but there are situations where the increased flexibility is instrumental in achieving the desired effect. Transactions mainly deal with the **I** in **ACID,** Isolation.*

Simply put, *[Partitioning](5.%20Distributed/Partitioning/Partitioning.md)* is a method for distributing data across multiple machines==*. Sharding becomes especially handy when no single machine can handle the expected workload.

**[Document database](Document%20database)** target use cases where data comes in self-constrained documents and relationships between one document and another are rare. 

**[Graph databases](Graph%20databases)** go in the opposite direction, targeting use cases where anything is potentially related to everything. 

# References:

1. ~~[Relational Databases Explained](https://architecturenotes.co/things-you-should-know-about-databases/)~~
2. **[SQLite Internals: Pages & B-trees](https://fly.io/blog/sqlite-internals-btree/)**
3. [**Как устроена виртуальная машина SQLite**](https://habr.com/ru/articles/687994/)
4. **[Relational database management system (RDBMS)](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)**
5. [~~Базы данных для программиста](https://www.youtube.com/playlist?list=PLmqFxxywkatS8Hfj6-aYgXfrpvV6OoKSc) (course, videos)~~
6. [Fundamentals of Database Engineering](https://www.udemy.com/course/database-engines-crash-course/?couponCode=DB-AUG2023-A) (course)
7. [Курс "Базы данных. Лаборатория Tarantool" (2018)](https://www.youtube.com/playlist?list=PLrCZzMib1e9o-2km1HniylB-ZZteznvLb) (course, videos)
8. Так что, мы советуем тем, кто хочет изучить эту область самостоятельно, избегать учебников и начать с записи курса по базам данных от Джо Хеллерстайна [CS 186 университета Беркли](https://www.youtube.com/user/CS186Berkeley/videos), а затем перейти к изучению статей по этой теме.
9. Одна из статей для новичков в этой области, которые стоит упомянуть, это “[Architecture of a Database System](http://db.cs.berkeley.edu/papers/fntdb07-architecture.pdf)”. Она хороша тем, что дает общие представления о том, как работают системы управления реляционными базами данных. Она послужит отличной основой для дальнейшего обучения.
10. Readings in Database Systems, больше известная как [“Красная книга”](http://www.redbook.io/)  - это коллекция статей, собранная и отредактированная Питером Бейлисом, Джо Хеллерстейном и Майклом Стоунбрейкером. Для тех, кто уже хорошо изучил курс CS 186, красная книга - следующий шаг.
11. [Database Internals: A Deep Dive into How Distributed Data Systems Work](http://libgen.rs/book/index.php?md5=902BA0835E8E3E0262C236E3198E1EF5) ([ru](http://libgen.rs/book/index.php?md5=7190A96149EE6C8585B7E8876305694A)) (book) + [Tinkoff Book Club](https://www.youtube.com/playlist?list=PLLrf_044z4JolqOL0jb_miJpRhFml6Ire)
13. [Foundations of Databases](http://webdam.inria.fr/Alice/#) (book)
14. [Базы данных: Проектирование, реализация и сопровождение: Теория и практика](http://library.lol/main/952FC069138E2A80E8237069487C886C) (book)
15. [MySQL по максимуму: оптимизация, репликация, резервное копирование](http://library.lol/main/A504CE89F0597C6E2FAB083B84525CE0) (book)
17. [Безопасность и СУБД: о чём надо помнить, подбирая средства защиты](https://habr.com/ru/company/vk/blog/500708/)