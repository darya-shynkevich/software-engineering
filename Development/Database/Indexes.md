*A database index functions as a specialized structure designed to accelerate the speed and efficiency of query operations within a database.*

*Read performance increases as you index the data, but that comes at the cost of write performance since you need to keep index up to date.*

![[Pasted image 20230605131446.png]]

***Index leaf nodes** are given a specific column to index, they can store the location of the matching row(s).* *These index leaf nodes are the mapping between the indexed column and where the corresponding row lives on the disk. This gives us a quick way to get to a specific row if you reference it by indexed column. Scanning the index can be much faster since it is a compact representation (fewer bytes) of the column you are searching by. It saves you time reading a bunch of blocks looking for the requested data and is much more convenient to cache, further speeding up the entire process.*

![[Pasted image 20230605132807.png]]


## ==*Scale of data often works against you, and balanced trees are the first tool in your arsenal against it.*==

###### Blocks

*In computing, **blocks** are a grouping of bytes that usually contain a fixed number of records which are limited by a total length (block length). So if we were to calculate the number of bytes it would take to store a row divided by the block length, it would give us how many rows could be read from a specific block.* 

*At a very low level you can use this to reason about how performant your systems can be. Quick Maths™ can be very powerful when you are capacity planning.*

*Since these leaf nodes aren't arranged physically on disk in order (remember pointers maintain the sorting in the doubly linked list), we need a way to get to the correct index leaf nodes.*

#### Balanced Trees (B-Trees)

![[Pasted image 20230605133326.png]]

***B+Trees*** allows us to build a tree structure where each intermediate node points to the highest node value of its respective leaf nodes. It ***gives us a clear path to find the index leaf node that will point to the necessary data***.

![[Pasted image 20230605133604.png]]

##### B-Tree vs. B+Tree

*The main difference **B+ Trees** show off is that **intermediate nodes don't store any data on them**. Instead, all the data references are linked to the leaf nodes, which allows for better caching of the tree structure.

*Secondly, the **leaf nodes are linked, so if you need to do an index scan, you can do a single linear pass** rather than traversing the entire tree up and down and loading more index data from the disk.*

#### Logarithmic Scalability


  