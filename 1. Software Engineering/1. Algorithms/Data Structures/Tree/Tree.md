
# References:

[Trees, Binary Trees, & Binary Search](https://www.youtube.com/playlist?list=PLiQ766zSC5jND9vxch5-zT7GuMigiWaV_) (videos) — различные видео по решению задач

Know least one type of balanced binary tree (and know how it's implemented):

"Among balanced search trees, AVL and 2/3 trees are now passé, and red-black trees seem to be more popular. A particularly interesting self-organizing data structure is the splay tree, which uses rotations to move any accessed key to the root." - Skiena

Of these, I chose to implement a splay tree. From what I've read, you won't implement a balanced search tree in your interview. But I wanted exposure to coding one up and let's face it, splay trees are the bee's knees. I did read a lot of red-black tree code.

1. splay tree: insert, search, delete functions If you end up implementing red/black tree try just these:
2. search and insertion functions, skipping delete
3. [How To Not Be Stumped By Trees](https://medium.com/basecs/how-to-not-be-stumped-by-trees-5f36208f68a7)
4. [Trying to Understand Tries](https://medium.com/basecs/trying-to-understand-tries-3ec6bede0014)
5. [Become Master in Tree](https://leetcode.com/discuss/study-guide/1820334/become-master-in-tree)
6. [Tree Algorithms](https://www.youtube.com/playlist?list=PLDV1Zeh2NRsDfGc8rbQ0_58oEZQVtvoIc) (videos)
7. [DFS, what is inorder, preorder, postorder traversal.](http://www.geeksforgeeks.org/tree-traversals-inorder-preorder-and-postorder/)
8. [BFS](http://algorithms.tutorialhorizon.com/breadth-first-searchtraversal-in-a-binary-tree/)
1. [Grokking the Coding Interview: Pattern Island](https://levelup.gitconnected.com/grokking-the-coding-interview-pattern-island-dc9c7def6b54)
9. [DFS vs BFS](http://www.geeksforgeeks.org/bfs-vs-dfs-binary-tree/)
10. [DFS iteratively](http://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion/)
11. [Compressing Radix Trees Without (Too Many) Tears](https://medium.com/basecs/compressing-radix-trees-without-too-many-tears-a2e658adb9a0)
12. [Finding Fibonacci In Golden Trees](https://medium.com/basecs/finding-fibonacci-in-golden-trees-1c8967b1f47a)
13. [Оптимизируем дерево отрезков, делаем из него куст o_O](https://habr.com/ru/articles/697598/)
14. [Суффиксное дерево на python](https://habr.com/ru/articles/681940/)
15. [Fenwick Tree/Binary](https://www.youtube.com/playlist?list=PLDV1Zeh2NRsCvoyP-bztk6uXAYoyZg_U9) (videos)
16. [Grammatically Rooting Oneself With Parse Trees](https://medium.com/basecs/grammatically-rooting-oneself-with-parse-trees-ec9daeda7dad)
17. [Leveling Up One’s Parsing Game With ASTs](https://medium.com/basecs/leveling-up-ones-parsing-game-with-asts-d7a6fc2400ff)
18. [Divide & Conquer: van Emde Boas Trees (video)](https://www.youtube.com/watch?v=hmReJCupbNU&list=PLUl4u3cNGP6317WaSNfmCvGym2ucw3oGp&index=6) + [MIT Lecture Notes](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-046j-design-and-analysis-of-algorithms-spring-2012/lecture-notes/MIT6_046JS12_lec15.pdf)
19. [Segment Tree 1: Basic Concepts and Operations in Detail](https://medium.com/@florian_algo/segment-tree-1-basic-concepts-and-operations-in-detail-19a38c69648e)


**Binary trees:**

1. [Структуры данных: бинарные деревья. Часть 1](https://habr.com/ru/post/65617/)
2. [Leaf It Up To Binary Trees](https://medium.com/basecs/leaf-it-up-to-binary-trees-11001aaf746d)
3. [Binary Search Tree Playlist](https://www.youtube.com/playlist?list=PLDV1Zeh2NRsCYY48kOkeLQ-cg9-eqInzs) (videos)
4. [Search and Insertion in Binary Search Tree](https://www.geeksforgeeks.org/binary-search-tree-set-1-search-and-insertion/)
5. [Binary Search Trees](https://algs4.cs.princeton.edu/32bst/)

**AVL trees:**

In practice: From what I can tell, these aren't used much in practice, but I could see where they would be: The AVL tree is another structure supporting O(log n) search, insertion, and removal. It is more rigidly balanced than red–black trees, leading to slower insertion and removal but faster retrieval. This makes it attractive for data structures that may be built once and loaded without reconstruction, such as language dictionaries (or program dictionaries, such as the opcodes of an assembler or interpreter).

1. [The Little AVL Tree That Could](https://medium.com/basecs/the-little-avl-tree-that-could-86a3cae410c7)
2. [AVL tree playlist](https://www.youtube.com/playlist?list=PLDV1Zeh2NRsD06x59fxczdWLhDDszUHKt) (videos)
3. [AVL Trees (video)](https://www.coursera.org/learn/data-structures/lecture/Qq5E0/avl-trees)
4. [AVL Tree Implementation (video)](https://www.coursera.org/learn/data-structures/lecture/PKEBC/avl-tree-implementation)
5. [Split And Merge](https://www.coursera.org/learn/data-structures/lecture/22BgE/split-and-merge)

**Splay trees:**

In practice: Splay trees are typically used in the implementation of caches, memory allocators, routers, garbage collectors, data compression, ropes (replacement of string used for long text strings), in Windows NT (in the virtual memory, networking and file system code) etc.

1. [CS 61B: Splay Trees (video)](https://archive.org/details/ucberkeley_webcast_G5QIXywcJlY)
2. MIT Lecture: Splay Trees:
    1. Gets very mathy, but watch the last 10 minutes for sure.
    2. [Video](https://www.youtube.com/watch?v=QnPl_Y6EqMo)

**Red/black trees:**

these are a translation of a 2-3 tree (see below)

In practice: Red–black trees offer worst-case guarantees for insertion time, deletion time, and search time. Not only does this make them valuable in time-sensitive applications such as real-time applications, but it makes them valuable building blocks in other data structures which provide worst-case guarantees; for example, many data structures used in computational geometry can be based on red–black trees, and the Completely Fair Scheduler used in current Linux kernels uses red–black trees. In the version 8 of Java, the Collection HashMap has been modified such that instead of using a LinkedList to store identical elements with poor hashcodes, a Red-Black tree is used.

1. [Painting Nodes Black With Red-Black Trees](https://medium.com/basecs/painting-nodes-black-with-red-black-trees-60eacb2be9a5)
2. [Балансировка красно-чёрных деревьев — Три случая](https://habr.com/ru/company/otus/blog/472040/)
3. [An Introduction To Binary Search And Red Black Tree](https://www.topcoder.com/community/data-science/data-science-tutorials/an-introduction-to-binary-search-and-red-black-trees/)
4. [Aduni - Algorithms - Lecture 4 (link jumps to starting point) (video)](https://youtu.be/1W3x0f_RmUo?list=PLFDnELG9dpVxQCxuD-9BSy2E7BWY3t5Sm&t=3871)
5. [Aduni - Algorithms - Lecture 5 (video)](https://www.youtube.com/watch?v=hm2GHwyKF1o&list=PLFDnELG9dpVxQCxuD-9BSy2E7BWY3t5Sm&index=5)

**2-3 search trees:**

In practice: 2-3 trees have faster inserts at the expense of slower searches (since height is more compared to AVL trees).

You would use 2-3 tree very rarely because its implementation involves different types of nodes. Instead, people use Red Black trees.

1. [23-Tree Intuition and Definition (video)](https://www.youtube.com/watch?v=C3SsdUqasD4&list=PLA5Lqm4uh9Bbq-E0ZnqTIa8LRaL77ica6&index=2)
2. [Binary View of 23-Tree](https://www.youtube.com/watch?v=iYvBtGKsqSg&index=3&list=PLA5Lqm4uh9Bbq-E0ZnqTIa8LRaL77ica6)
3. [2-3 Trees (student recitation) (video)](https://www.youtube.com/watch?v=TOb1tuEZ2X4&index=5&list=PLUl4u3cNGP6317WaSNfmCvGym2ucw3oGp)

**2-3-4 Trees (aka 2-4 trees):**

In practice: For every 2-4 tree, there are corresponding red–black trees with data elements in the same order. The insertion and deletion operations on 2-4 trees are also equivalent to color-flipping and rotations in red–black trees. This makes 2-4 trees an important tool for understanding the logic behind red–black trees, and this is why many introductory algorithm texts introduce 2-4 trees just before red–black trees, even though 2-4 trees are not often used in practice.

1. [CS 61B Lecture 26: Balanced Search Trees (video)](https://archive.org/details/ucberkeley_webcast_zqrqYXkth6Q)
2. [Bottom Up 234-Trees (video)](https://www.youtube.com/watch?v=DQdMYevEyE4&index=4&list=PLA5Lqm4uh9Bbq-E0ZnqTIa8LRaL77ica6)
3. [Top Down 234-Trees (video)](https://www.youtube.com/watch?v=2679VQ26Fp4&list=PLA5Lqm4uh9Bbq-E0ZnqTIa8LRaL77ica6&index=5)

**N-ary (K-ary, M-ary) trees:**

note: the N or K is the branching factor (max branches)

binary trees are a 2-ary tree, with branching factor = 2

1. 2-3 trees are 3-ary
2. [K-Ary Tree](https://en.wikipedia.org/wiki/K-ary_tree)

**B-Trees:**

In Practice: B-Trees are widely used in databases. Most modern filesystems use B-trees (or Variants). In addition to its use in databases, the B-tree is also used in filesystems to allow quick random access to an arbitrary block in a particular file. The basic problem is turning the file block i address into a disk block (or perhaps to a cylinder-head-sector) address.

1. [B-tree](https://habr.com/ru/articles/114154/)
2. [Introduction of B-Tree](https://www.geeksforgeeks.org/introduction-of-b-tree-2/)
3. [Busying Oneself With B-Trees](https://medium.com/basecs/busying-oneself-with-b-trees-78bbf10522e7)
4. [B-Tree](https://en.wikipedia.org/wiki/B-tree) (wikipedia)
5. [Introduction to B-Trees (video)](https://www.youtube.com/watch?v=I22wEC1tTGo&list=PLA5Lqm4uh9Bbq-E0ZnqTIa8LRaL77ica6&index=6)
6. [B-Tree Definition and Insertion (video)](https://www.youtube.com/watch?v=s3bCdZGrgpA&index=7&list=PLA5Lqm4uh9Bbq-E0ZnqTIa8LRaL77ica6)
7. [B-Tree Deletion (video)](https://www.youtube.com/watch?v=svfnVhJOfMc&index=8&list=PLA5Lqm4uh9Bbq-E0ZnqTIa8LRaL77ica6)
8. [MIT 6.851 - Memory Hierarchy Models (video)](https://www.youtube.com/watch?v=V3omVLzI0WE&index=7&list=PLUl4u3cNGP61hsJNdULdudlRL493b-XZf) - covers cache-oblivious B-Trees, very interesting data structures - the first 37 minutes are very technical, may be skipped (B is block size, cache line size)

**k-D Trees:**

great for finding number of points in a rectangle or higher dimension object

a good fit for k-nearest neighbors

1. [Kd Trees (video)](https://www.youtube.com/watch?v=W94M9D_yXKk)
2. [kNN K-d tree algorithm (video)](https://www.youtube.com/watch?v=Y4ZgLlDfKDg)
3. [The k-Nearest Neighbors (kNN) Algorithm in Python](https://realpython.com/knn-python/)
4. [Ищем максимальную разницу между соседями. User-friendly-разбор задачи по алгоритмам](https://habr.com/ru/company/yandex_praktikum/blog/531748/)