***==multi-level link list==***

***==A better concurrency friendly data structure than B Tree.==***

B Tree are great, BUT they are:
1. complex;
2. costly, when dealing with rebalancing;
3. even more complex under multi-processing (threading) env

So the advantages of using `SkipList` are:
	(+) speed
	(+) logN for all operations (search, insert, delete)
	(+) no balancing cost
	(+) easier to implement and relative cheaper to adjust the links (which is more friendly to parellel processing).
	
(-) The trade off is the need of extra (1+N)*N/2 space to store the links for each level .

SkipList is ***a data structure*** that allows for efficient searching, insertion, and deletion operations in a sorted list of elements. It achieves this by ***using multiple layers of linked lists with a probabilistic mechanism to bypass a variable number of elements, hence the name "skip."*** 

Attributes of skip list:
1. keys in sorted order.
2. O(log n) levels
3. each higher level contains 1/2 the elements of the level below

![Pasted image 20231019131557](../../../_Attachments/Pasted%20image%2020231019131557.png)

Here's a step-by-step explanation of how `SkipList` works:
1. The `SkipList` consists of nodes, each containing a value and a set of forward `pointers`. The number of forward `pointers` in each node is determined probabilistically, usually through a coin flip.
2. The bottom layer of the SkipList is a traditional `sorted linked list`, where nodes are arranged in `ascending order` based on their values.
3. Above the base layer, additional layers are created, with each layer ***skipping a varying number of elements***. These additional layers provide shortcuts to quickly traverse the list.
4. The ***top layer*** contains only two nodes: a `head node` and a `tail node`. The `head node` points to the ***first element in the bottom layer***, and the `tail node` points to ***the last element***.

# Operations
## Search
Searching in a `SkipList` starts from the top layer and moves downwards. At each layer, the forward pointer is followed until the desired value is found or exceeded. If exceeded, the search moves to the next layer, and the process continues until the value is found or determined to be absent.

Average: O(logN)
Worst case: O(N)


![Pasted image 20231019132106](../../../_Attachments/Pasted%20image%2020231019132106.png)

Max visit log(N) levels then find the value : when value < current, go next level; otherwise check next node.

## Insert / Delete
Insertion and deletion operations involve searching for the appropriate position in the `SkipList` using the same technique as searching. Once the position is found, the new node is inserted or the existing node is removed, with the forward pointers adjusted accordingly.

Average: O(logN)
Worst case: O(N)

1. Locate the index to insert
![Pasted image 20231019132304](../../../_Attachments/Pasted%20image%2020231019132304.png)
2. Build the link for each level (adjustment) after insert/delete (and rebuild links):
   ![Pasted image 20231019132504](../../../_Attachments/Pasted%20image%2020231019132504.png)

# Use cases:
1. [MemSQL](https://en.wikipedia.org/wiki/MemSQL) [Lucene](https://en.wikipedia.org/wiki/Lucene)
2. [Redis](https://en.wikipedia.org/wiki/Redis)
3. More can be found : [https://en.wikipedia.org/wiki/Skip_list](https://en.wikipedia.org/wiki/Skip_list)


# References:
1. [Those Data structures can not be learned from leetcode- Skiplist](https://iorilan.medium.com/those-data-structures-can-not-be-learned-from-leetcode-skiplist-2b592d11e307)
2. These are somewhat of a cult data structure" - Skiena
3. [Randomization: Skip Lists (video)](https://www.youtube.com/watch?v=2g9OSRKJuzM&index=10&list=PLUl4u3cNGP6317WaSNfmCvGym2ucw3oGp)
4. [For animations and a little more detail](https://en.wikipedia.org/wiki/Skip_list)