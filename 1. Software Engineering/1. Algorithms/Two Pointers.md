## ==*Two are better than one if they act as one.*==

*This approach is generally used to search pairs in a sorted array. This approach works in constant space.*

1. One slow-runner and the other fast-runner.
2. One pointer starts from the beginning while the other pointer starts from the end.

A classic example is to remove duplicates from a sorted array, which is available for you to practice [here](https://leetcode.com/problems/remove-duplicates-from-sorted-array/).

Common patterns in the two-pointer approach entail:

1. Two pointers, each starting from the beginning and the end until they both meet.
2. One pointer moving at a slow pace, while the other pointer moves at twice the speed.
3. Binary Search
4. Slow and Fast Pointer to detect a cycle.
5. One pointer as a boundary and one pointer to search.
6. Pointer at the beginning of two sorted arrays.
7. Sliding Window (Advanced)

###### Recognise It

There are a few ways we can use the Two Pointer approach. First, if the input is an array or string, we should keep Two Pointers (along with hash tables) on our tool belt.

*==Stronger signals for Two Pointers are….==*
1. If we want to reduce nested loops to a single pass.
2. If the input is sequenced or sorted.
3. If we need to compare a value at one index with value at another index.
4. If we need to make swaps between indices.
5. If we partition the array.


**References:**
1. [Leetcode is Easy! The Two Pointer Pattern]([https://medium.com/@timpark0807/leetcode-is-easy-two-pointers-90b9b0f2eb43](https://www.google.com/url?q=https://medium.com/@timpark0807/leetcode-is-easy-two-pointers-90b9b0f2eb43&sa=D&source=calendar&usd=2&usg=AOvVaw3HOscey4eQ6T6lcrlqObK2)  )
2. [Two Pointers Approach — Python Code](https://towardsdatascience.com/two-pointer-approach-python-code-f3986b602640)