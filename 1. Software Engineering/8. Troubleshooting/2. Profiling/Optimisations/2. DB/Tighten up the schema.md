
- MySQL dumps to disk in contiguous blocks for fast access.
- Use `CHAR` instead of `VARCHAR` for fixed-length fields.
    - `CHAR` effectively allows for fast, random access, whereas with `VARCHAR`, you must find the end of a string before moving onto the next one.
- Use `TEXT` for large blocks of text such as blog posts. `TEXT` also allows for boolean searches. Using a `TEXT` field results in storing a pointer on disk that is used to locate the text block.
- Use `INT` for larger numbers up to 2^32 or 4 billion.
- Use `DECIMAL` for currency to avoid floating point representation errors.
- Avoid storing large `BLOBS`, store the location of where to get the object instead.
- `VARCHAR(255)` is the largest number of characters that can be counted in an 8 bit number, often maximizing the use of a byte in some RDBMS.
- Set the `NOT NULL` constraint where applicable to [improve search performance](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search).
- [Avoid B-indexes on UUID fileds](UUID%20and%20DB%20performance.md) => Use HASH indexes for UUID fields 
- Do not use [Offsets](Offsets.md)

# References:

1. [Tips for optimizing MySQL queries](http://aiddroid.com/10-tips-optimizing-mysql-queries-dont-suck/)
2. [Is there a good reason i see VARCHAR(255) used so often?](http://stackoverflow.com/questions/1217466/is-there-a-good-reason-i-see-varchar255-used-so-often-as-opposed-to-another-l)
3. [Оптимизация поиска по большому полю](https://habr.com/ru/companies/kaspersky/articles/705780/)
4. [How do null values affect performance?](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search)
5. [Slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)
6. [Can NULLs Improve your Database Queries Performance](https://www.youtube.com/watch?v=iSwJI00Rv2s&list=PLQnljOFTspQUybacGRk1b_p13dgI-SmcZ&index=38) (video)
7. [Is SELECT * Expensive?](https://www.youtube.com/watch?v=QQVNVOneZNg&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=10)
8. [SELECT COUNT (start) can impact your Backend Application Performance, here is why](https://www.youtube.com/watch?v=8xKS7QQKgzk&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=4) (video)
9. [SELECT COUNT(star) is Slow, Estimate it Instead (with Example in Node JS and Postgres)](https://www.youtube.com/watch?v=eI_EQNTxF6U&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=69) (video)
10. [Understanding Aggregate Functions Performance | The Backend Engineering Show](https://www.youtube.com/watch?v=L-8_CjV6sH4&list=PLQnljOFTspQUybacGRk1b_p13dgI-SmcZ&index=57) (video)
11. [Why this query is fast](https://www.youtube.com/watch?v=HinCxBt6mNY&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=94) (video)
12. [What is the cost of Indexing too many columns](https://www.youtube.com/watch?v=YeYIxbiupoo&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=90) (video)
13. ~~[When indexes are useless | The Backend Engineering Show](https://www.youtube.com/watch?v=oebtXK16WuU&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=89) (video)~~
14. [Watch out before Adding Indexes to Your Table, Your Database Optimizer Might not Use them](https://www.youtube.com/watch?v=zu97H24zpXU&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=66) (video)
15. [The cost rolling back transactions (postgres/mysql)](https://www.youtube.com/watch?v=omizQEkTcl4&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=88) (video)
16. [Create Index Blocking Production Database Writes? Postgres Solves this with this trick](https://www.youtube.com/watch?v=Fvv6eEDi66w&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=59) (video)