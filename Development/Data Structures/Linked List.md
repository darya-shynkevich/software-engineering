`Linked lists` are an ordered collection of objects; they are differ from `lists` in the way that they store elements in memory. While `lists` use a contiguous memory block to store references to their data, `linked lists` store references as part of their own elements.

Each element of a linked list is called a **node**, and every node has two different fields:
1. **Data** contains the value to be stored in the node.
2. **Next** contains a reference to the next node on the list.

*A node only knows about what data it contains, and who its neighbor is.*

A `linked list` is a collection of nodes. The first node is called the `head`, and it’s used as the starting point for any iteration through the list. The last node must have its `next` reference pointing to [`None`](https://realpython.com/null-in-python/) to determine the end of the list.

![[Pasted image 20230604195242.png]]

#### Practical Applications

They can be used to implement queues or [stacks](https://realpython.com/how-to-implement-python-stack/) as well as graphs. They’re also useful for much more complex tasks, such as lifecycle management for an operating system application.

1. Queues:  **First-In/First-Out** (FIFO)
2. Stack: **Last-In/Fist-Out** (LIFO)
3. Graphs: **adjacency list**. An adjacency list is, in essence, a list of linked lists where each vertex of the graph is stored alongside a collection of connected vertices

![[Pasted image 20230604190411.png]]

```
graph = {
...     1: [2, 3, None],
...     2: [4, None],
...     3: [None],
...     4: [5, 6, None],
...     5: [6, None],
...     6: [None]
... }
```

*==a linked list is usually efficient when it comes to adding and removing most elements, but can be very slow to search and find a single element==.*

##### Performance Comparison: Lists vs Linked Lists

In most programming languages, there are clear differences in the way linked lists and arrays are stored in memory. 

![[Pasted image 20230604195502.png]]

![[Pasted image 20230604223826.png]]

In Python, however, lists are [dynamic arrays](https://docs.python.org/3.7/faq/design.html#how-are-lists-implemented-in-cpython). That means that the memory usage of both lists and linked lists is very similar.

*CPython’s lists are really variable-length arrays, not Lisp-style linked lists. The implementation uses a contiguous array of references to other objects, and keeps a pointer to this array and the array’s length in a list head structure.

*This makes indexing a list `a[i]` an operation whose cost is independent of the size of the list or the value of the index.

*When items are appended or inserted, the array of references is resized. Some cleverness is applied to improve the performance of appending items repeatedly; when the array must be grown, some extra space is allocated so the next few times don’t require an actual resize.*

Python’s [[Implementation Of Dynamic Arrays]]

![[Pasted image 20230604191340.png]]


###### Insertion and Deletion of Elements

`.insert()` and `.remove()` - to insert or remove elements at a specific position in a list
`.append()` and `.pop()`  - only to insert or remove elements at the end of a list.

**`collections.deque`**

Create a linked list: any iterable as an input, such as a string (also an iterable) or a list of objects

```
>>> deque(['a','b','c'])
deque(['a', 'b', 'c'])

>>> deque('abc')
deque(['a', 'b', 'c'])

>>> deque([{'data': 'a'}, {'data': 'b'}])
deque([{'data': 'a'}, {'data': 'b'}])
```

Inserting new element:

```
>>> llist = deque("abcde")
>>> llist
deque(['a', 'b', 'c', 'd', 'e'])

>>> llist.append("f")
>>> llist
deque(['a', 'b', 'c', 'd', 'e', 'f'])

>>> llist.pop()
'f'

>>> llist
deque(['a', 'b', 'c', 'd', 'e'])
```

Use `deque` to quickly add or remove elements from the left side, or `head`, of the list

```
>>> llist.appendleft("z")
>>> llist
deque(['z', 'a', 'b', 'c', 'd', 'e'])

>>> llist.popleft()
'z'

>>> llist
deque(['a', 'b', 'c', 'd', 'e'])
```


###### Implementing Your Own Linked List

```
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

    def __repr__(self):
        return self.data

class LinkedList:
    def __init__(self):
        self.head = None

    def __repr__(self):
        node = self.head
        nodes = []
        
        while node is not None:
            nodes.append(node.data)
            node = node.next
            
        nodes.append("None")
        return " -> ".join(nodes)
```

###### How to Traverse a Linked List

```
def __iter__(self):
    node = self.head
    while node is not None:
        yield node
        node = node.next
```

###### How to Insert a New Node

1. **Inserting at the Beginning**

```
def add_first(self, node):
    node.next = self.head
    self.head = node
```

2. **Inserting at the End**

```
def add_last(self, node):
    if self.head is None:
        self.head = node
        return
        
    for current_node.next is None:
		current_node.next = node
```

3. **Inserting Between Two Nodes**

```
def add_after(self, target_node_data, new_node):
    if self.head is None:
        raise Exception("List is empty")

    for node in self:
        if node.data == target_node_data:
            new_node.next = node.next
            node.next = new_node
            return
```

```
def add_before(self, target_node_data, new_node):  
    if self.head is None:  
        raise Exception("List is empty")  
  
    if self.head.data == target_node_data:  
        return self.add_first(new_node)  
  
    prev_node = self.head  
    for node in self:  
        if node.data == target_node_data:  
            prev_node.next = new_node  
            new_node.next = node  
            return  
            
        prev_node = node
```

###### How to Remove a Node

```
def remove_node(self, target_node_data):
     if self.head is None:
         raise Exception("List is empty")
 
     if self.head.data == target_node_data:
         self.head = self.head.next
         return
 
    previous_node = self.head
    for node in self:
        if node.data == target_node_data:
            previous_node.next = node.next
            return
            
        previous_node = node
```


##### How to Use Doubly Linked Lists

Doubly linked lists are different from singly linked lists in that they have two references:

1. The `previous` field references the previous node.
2. The `next` field references the next node.

*For example, if we wanted to be able to hop between one node and the node previous without having to go back to the _very beginning of the list_, a doubly linked list would be a better data structure than a singly linked list. However, everything requires space and memory, so if our node had to store two reference pointers instead of just one, that would be another thing to consider.*

![[Pasted image 20230604194012.png]]

```
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
        self.previous = None
```


##### How to Use Circular Linked Lists

**Circular linked lists** are a type of linked list in which the last node points back to the `head` of the list instead of pointing to `None`. This is what makes them circular. Circular linked lists have quite a few interesting use cases:

- Going around each player’s turn in a multiplayer game
- Managing the application life cycle of a given operating system
- Implementing a [Fibonacci heap](https://en.wikipedia.org/wiki/Fibonacci_heap)

![[Pasted image 20230604194152.png]]

One of the advantages of circular linked lists is that you can traverse the whole list starting at any node.


References:
1. *https://en.wikipedia.org/wiki/Linked_list
2. [_Linked Lists in Python: An Introduction_](https://realpython.com/linked-lists-python/)
3. [_What’s a Linked List, Anyway?_](https://medium.com/basecs/whats-a-linked-list-anyway-part-1-d8b7e6508b9d)
4. [_Core Linked Lists Vs Arrays (video)_](https://www.coursera.org/learn/data-structures-optimizing-performance/lecture/rjBs9/core-linked-lists-vs-arrays)
5. [_In The Real World Linked Lists Vs Arrays (video)_](https://www.coursera.org/learn/data-structures-optimizing-performance/lecture/QUaUd/in-the-real-world-lists-vs-arrays)
6. [_why you should avoid linked lists (video)_](https://www.youtube.com/watch?v=YQs6IC-vgmo) *Bjarne Stroustrup, the creator of C++, explains in a video that vectors are better than linked lists for frequent insertions and deletions due to the linear search needed for finding insertion points in linked lists, which can be much slower than with vectors. The extra space needed for linked lists and their unpredictable usage patterns also make them less efficient. Stroustrup emphasizes the significance of data structure selection on performance and recommends using compact data structures on the free store to optimize performance.*
