
### `sqlite`

Data is stored in a single file => your application is running on one machine and one machine only. Or at least one shared filesystem. 

### [`DynamoDB`](Amazon%20DynamoDB.md), [`Cassandra`](Cassandra.md), or [`MongoDB`](MongoDB.md) 

A giant distributed hash map. The only operations that work without needing to scan the entire database are lookups by partition key and scans that make use of a sort key.

   You should use them if
	1. You know exactly what your app needs to do, up-front
	2. You know exactly what your access patterns will be, up-front
	3. You have a known need to scale to really large sizes of data
	4. You are okay giving up some level of consistency

Whatever queries you need to make, you need to encode that knowledge in one of those indexes before you store it. You want to store users and look them up by either first name or last name? Well you best have a sort key that looks like `<FIRST NAME>$<LAST NAME>`. Your access patterns should be baked into how you store your data. If your access patterns change significantly, you might need to reprocess all of your data.

### `Valkey`

The artist formerly known as [`Redis`](1.%20Software%20Engineering/2.%20Architecture/2.%20Components/Cache/Types/Redis/_Base.md) is best known for being an efficient out-of-process cache. You compute something expensive once and slap it in `Valkey` so all 5 or so HTTP servers you have don't need to recompute it.

However, you _can_ use it as your primary database. It stores all its data in RAM, so it's pretty fast if you do that.

Same as the `DynamoDB`-likes, you need to make concessions on how you model your data.

### `Datomic`

You don't store data in tables. It's all "entity-attribute-value-time" (EAVT) pairs. Instead of a person row with `id`, `name`, and `age` you store `1 :person/name "Beth"` and `1 :person/age 30`. Then your queries work off of "universal" indexes.

You don't need to coordinate with writers when making queries. You query the database "as-of" a given time. New data, even deletions (or as they call them "retractions"), don't actually delete old data.

But there are some significant problems
- It only works with JVM languages.
- Outside of `Clojure`, a relatively niche language, its API sucks.
- If you structure a query badly the error messages you get are terrible.
- The whole universe of tools that exist for SQL just aren't there.

### `XTDB`

`XTDB` is spiritually similar do `Datomic` but:
- There is an HTTP api, so you aren't locked to the JVM.
- It has two axes of time you can query against. "System Time" - when records were inserted - and "Valid Time."
- It has a SQL API.

The biggest points against it are:
- It's new. Its SQL API is something that popped up in the last year. It recently changed its whole storage model. Will the company behind it survive the next 10 years? Who knows!

### `Kafka`

`Kafka` is an append only log. It can handle TBs of data. It is a very good append only log. It works amazingly well if you want to do event sourcing type stuff with data flowing in from multiple services maintained by multiple teams of humans.

But:
- Up to a certain scale, a table in Postgres works perfectly fine as an append only log.
- You likely do not have hundreds of people working on your product nor TBs of events flowing in.
- Making a Kafka consumer is a bit more error-prone than you'd expect. You need to keep track of your place in the log after all.
- Even when maintained by a cloud provider (and there are good managed `Kafka` services) its another piece of infrastructure you need to monitor.

### [`ElasticSearch`](Elasticsearch)

Is searching over data the primary function of your product?

If yes, `ElasticSearch` is going to give you some real pros. You will need to ETL your data into it and manage that whole process, but `ElasticSearch` is built for searching. It does searching good.

If no, `Postgres` will be fine. A sprinkling of `ilike` and the built-in [full text search](https://www.postgresql.org/docs/current/textsearch.html) is more than enough for most applications. You can always bolt on a dedicated search thing later.

### [[MSSQL]] or [[Oracle DB]]

Genuine question you should ask yourself: Are these worth the price tag?

If you don't have some killer `MSSQL` feature that you simply cannot live without, don't use it.

### [`MySQL`](MySQL.md)

???

### [[MariaDB]]

1. it's on all the budget hosting services, so it is everywhere.
2. It's got the same slick indexing scheme as MSSQL -- clustered indexes -- and so can be made very performant.
3. Its team is adding objects, like SEQUENCEs, to help people escape Oracle's clutches. And other active development is happening.

### AI vector DB

- Most are new. Remember the risks of using something new.
- AI is a bubble. A load-bearing bubble, but a bubble. Don't build a house on it if you can avoid it.
- Even if your business is another AI grift, you probably only need to `import openai`.



# References:

1. [Just use Postgres](https://mccue.dev/pages/8-16-24-just-use-postgres)