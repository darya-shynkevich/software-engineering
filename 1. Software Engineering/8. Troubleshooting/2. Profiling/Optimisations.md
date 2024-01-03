# 1. Use a better algorithm.

! The selection sort algorithm above is faster than the bubble sort for random data, but the bubble sort is much faster for sorted data. ***The relationship between inputs and algorithmic performance can be subtle***. Famously, if you choose an unfortunate “pivot” when implementing [quicksort](https://en.wikipedia.org/wiki/Quicksort), you’ll find that it is very non-quick (e.g. you can make it as slow on already-sorted data as the selection sort above).

=> “use a better algorithm” requires understanding the wider context of your system and the nature of the algorithm you’re thinking of using.

# 2. Use a better data-structure

Just as with “use a better algorithm”, “use a better data-structure” requires careful thought and measurement.

! There is an important tactical variant on “better data-structures” that is perhaps best thought of as ***“put your structs/classes on a diet”***.
	1. reducing objects size
	2. reducing “pointer chasing” (typically by folding multiple structs/classes into one)
	3. encouraging memory locality <= it’s difficult to measure the indirect impact

# 3. Use a lower-level system

1. check that you’ve got compiler optimisations turned on, 
2. use faster libraries or databases

Sometimes rewriting in a lower-level programming language really is the right thing to do, but it is rarely a quick job, and it inevitably ***introduces a period of instability while bugs are shaken out of the new version.***

# 4. Accept a less precise solution

[local search](https://en.wikipedia.org/wiki/Local_search_(optimization)). => we define a metric (in our running example, how fast our benchmark suite runs) that allows us to compare two solutions (in our case, faster is better) and discard the worst. 
	1. [fast inverse square root](https://en.wikipedia.org/wiki/Fast_inverse_square_root) approximates multiplicative inverse
	2. [Bloom filter](https://en.wikipedia.org/wiki/Bloom_filter) can give false positives: accepting that possibility allows it to be exceptionally frugal with memory
	3. JPEG image compression deliberately throws away some of an image’s fine details in order to make the image more compressible

***Unless you are convinced you can tolerate incorrectness, you’re best off assuming that you can’t.***
# References:

1. ~~[Four Kinds of Optimisation](https://tratt.net/laurie/blog/2023/four_kinds_of_optimisation.html)~~