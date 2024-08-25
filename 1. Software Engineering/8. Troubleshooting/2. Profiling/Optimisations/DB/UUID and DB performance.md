If index is not fitted in a page => page split (that is a long operation)

It is better to use [ULID](https://cran.r-project.org/web/packages/ulid/vignettes/intro-to-ulid.html) because UUID are random and downgrade performance due to random access to index / disk. There are huge problems during INSERT operations (index page splitting / constant B+tree rebalancing)

# References:

1. ~~[The effect of Random UUID on database performance](https://www.youtube.com/watch?v=OAOQ7U0XAi0&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=111) (video)~~
2. [UUIDs are Bad for Performance in MySQL - Is Postgres better?](https://www.youtube.com/watch?v=Y5mWz4vK10A&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=61) (video)
3. [Unexpected Downsides Of UUID Keys In PostgreSQL](https://www.cybertec-postgresql.com/en/unexpected-downsides-of-uuid-keys-in-postgresql/)
4. [How Shopify’s engineering improved database writes by 50% with ULID](https://www.youtube.com/watch?v=f53-Iw_5ucA) (video)