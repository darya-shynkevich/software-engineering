Transaction concurrency problems can be solved by [[Transaction Loks]].

BUT we forget about one very important thing in the database — performance.

So, [[MVCC]] is a lock-free solution to track multiple versions (snapshots) of records to achieve high performance and concurrency safety.

















