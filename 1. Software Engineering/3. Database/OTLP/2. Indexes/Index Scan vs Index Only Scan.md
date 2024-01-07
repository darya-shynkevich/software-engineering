# Index Scan

![](../../../../_Attachments/Pasted%20image%2020240107135018.png)

We had to go back to the table on disk to fetch data (name)

# Index Only Scan

![](../../../../_Attachments/Pasted%20image%2020240107135230.png)

We do not have to go to the table because we fetch data from the index

## INCLUDE (PostgreSQL)

![](../../../../_Attachments/Pasted%20image%2020240107135518.png)

# Bitmap Index Scan (PostgreSQL)

![](../../../../_Attachments/Pasted%20image%2020240107185418.png)

You can think of a bitmap index scan as a middle ground between a sequential scan and an index scan. Like an index scan, ***it scans an index to determine exactly what data it needs to fetch, but like a sequential scan, it takes advantage of data being easier to read in bulk.***

The bitmap index scan actually operates in tandem with a Bitmap Heap Scan: it ***does not fetch the data itself***. Instead of producing the rows directly, the bitmap index scan ***constructs a bitmap of potential row locations***. It feeds this data to a parent Bitmap Heap Scan, which can decode the bitmap to fetch the underlying data, grabbing data page by page.

A Bitmap Heap Scan is the most common parent node of a Bitmap Index Scan, but a plan may also combine several different Bitmap Index Scans with BitmapAnd or BitmapOr nodes before actually fetching the underlying data. This allows Postgres to use two different indexes at once to execute a query.

![](../../../../_Attachments/Pasted%20image%2020240107185858.png)

# References:

1. ~~[Index Scan vs Index Only Scan on Database Systems (with Postgres)](https://www.youtube.com/watch?v=xsPBT5gIQac&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=49) (video)~~
2. ~~[Bitmap Index Scan in Postgres Explained with Examples](https://www.youtube.com/watch?v=rJ-oG0y1hqE&list=PLQnljOFTspQXjD0HOzN7P2tgzu7scWpl2&index=61) (video)~~
