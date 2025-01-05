The speed at which data can be ingested into a database directly impacts its utility and responsiveness, especially when **reaction speed** in real-time or near-real-time data processing is essential.

### 1. Use indexes in moderation

[Having the right indexes](https://www.timescale.com/learn/postgresql-performance-tuning-optimizing-database-indexes?ref=timescale.com) can speed up your queries, but they’re not a silver bullet. Incrementally maintaining indexes with each new row requires additional work. Check the number of indexes you’ve defined on your table (use the `psql` command `\d table_name`), and determine whether their potential query benefits outweigh the storage and insert overhead.

### 2. Reconsider foreign key constraints

Sometimes, it's necessary to build [foreign keys (FK)](https://www.postgresql.org/docs/current/tutorial-fk.html?ref=timescale.com) from one table to other relational tables. When you have an FK constraint, every `INSERT` will typically need to read from your referenced table, which can degrade performance. Consider denormalizing your data—we sometimes see pretty extreme use of FK constraints from a sense of “elegance” rather than engineering trade-offs.

### 3. Avoid unnecessary UNIQUE keys

Many use cases—including common monitoring or time-series applications—don’t require PK, as each event or sensor reading can simply be logged as a separate event by inserting it at the tail of a hypertable's current chunk during write time.

If a `UNIQUE` constraint is otherwise defined, that insert can necessitate an index lookup to determine if the row already exists, which will adversely impact the speed of your `INSERT`.

### 4. Use separate disks for WAL and data

While this is a more advanced optimization that isn't always needed, if your disk becomes a bottleneck, you can further increase throughput by using a separate disk (tablespace) for the database's WAL and data.

### 5. Use performant disks

Because when you insert rows, the data is durably stored in the WAL before the transaction completes, slow disks can impact insert performance.

One thing to do is check your disk IOPS using the `ioping` command.

```bash
// Read test:
ioping -q -c 10 -s 8k .

Write test:
ioping -q -c 10 -s 8k -W .
```

You should see at least thousands of read IOPS and many hundreds of write IOPS. If you are seeing far fewer, your disk hardware is likely affecting your INSERT performance. See if alternative storage configurations are feasible.

### 6. Use parallel writes

To achieve higher ingest, you should execute multiple `INSERT` or `COPY` commands in parallel.

### 7. Insert rows in batches

To achieve higher ingest rates, you should insert your data with many rows in each `INSERT` call (or else use some bulk insert command, like COPY or our parallel copy tool).

Don't insert your data row-by-row—instead, try at least hundreds (or thousands) of rows per insert. This allows the database to spend less time on connection management, transaction overhead, SQL parsing, etc., and more time on data processing.

### 8. Properly configure shared_buffers

We typically recommend 25 % of available RAM. If you install [[TimescaleDB]] via a method that runs [`timescaledb-tune`](https://github.com/timescale/timescaledb-tune?ref=timescale.com), it should automatically configure `shared_buffers` to something well-suited to your hardware specs.

! Note: in some cases, typically with virtualization and constrained cgroups memory allocation, these automatically-configured settings may not be ideal. To check that your `shared_buffers` are set to within the 25 % range, run `SHOW shared_buffers` from your `psql` connection.

### 9. Run Docker images on Linux hosts

If you are [running a TimescaleDB Docker container (which runs Linux)](https://docs.timescale.com/latest/getting-started/installation/docker/installation-docker/?utm_source=timescale-13-insert-tips&utm_medium=blog&utm_campaign=july-2020-advocacy&utm_content=install-docs-docker) on top of another Linux operating system, you're in great shape. The container is basically providing process isolation, and the overhead is extremely minimal.

If you're running the container on a Mac or Windows machine, you'll see some performance hits for the OS virtualization, including for I/O.

Instead, if you need to run on Mac or Windows, we recommend [installing directly](https://docs.timescale.com/latest/getting-started/installation/?utm_source=timescale-13-insert-tips&utm_medium=blog&utm_campaign=july-2020-advocacy&utm_content=install-docs) instead of using a Docker image.

### 10. Watch row width

The overhead from inserting a wide row (say, 50, 100, 250 columns) is going to be much higher than inserting a narrower row (more network I/O, more parsing and data processing, larger writes to WAL, etc.). Most of our published benchmarks are using [TSBS](https://github.com/timescale/tsbs?ref=timescale.com), which uses 12 columns per row. So you'll correspondingly see lower insert rates if you have very wide rows.

If you are considering very wide rows because you have different types of records, and each type has a disjoint set of columns, you might want to try using multiple hypertables (one per record type)—particularly if you don't often query across these types.

Additionally, JSONB records are another good option if virtually all columns are sparse. That said, if you're using sparse wide rows, use NULLs for missing records whenever possible, not default values, for the most performance gains (NULLs are much cheaper to store and query).

# References:

1. ~~[13 Tips to Improve PostgreSQL Insert Performance](https://www.timescale.com/blog/13-tips-to-improve-postgresql-insert-performance/)~~