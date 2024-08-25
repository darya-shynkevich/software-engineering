
**Note:** The Python interpreter is a virtual machine that executes the bytecode.

```Python
def my_function(): 
	for i in range(100000000): pass
```

The bytecode for this function might look like:

```Assembler
  SETUP_LOOP              20 (to 23)
  LOAD_GLOBAL             0 (range)
  LOAD_CONST              3 (100000000)
  CALL_FUNCTION           1
  GET_ITER            
  FOR_ITER                6 (to 22)
  STORE_FAST              0 (i)
  JUMP_ABSOLUTE           13
  POP_BLOCK           
  LOAD_CONST              0 (None)
  RETURN_VALUE
```

For script where the loop is at the top level of a Python:

```Assebmler
  SETUP_LOOP              20 (to 23)
  LOAD_NAME               0 (range)
  LOAD_CONST              3 (100000000)
  CALL_FUNCTION           1
  GET_ITER            
  FOR_ITER                6 (to 22)
  STORE_NAME              1 (i)
  JUMP_ABSOLUTE           13
  POP_BLOCK           
  LOAD_CONST              2 (None)
  RETURN_VALUE
```

! The `STORE_NAME` instruction is used here, rather than `STORE_FAST`.

The bytecode `STORE_FAST` is faster than `STORE_NAME` because *==in a function,==* ==*local variables are stored in a fixed-size array, not a dictionary.*== This array is directly accessible via an index, making variable retrieval very quick. Basically, it's just a pointer lookup into the list and an increase in the reference count of the PyObject, both of which are highly efficient operations.

On the other hand, global variables are stored in a dictionary. When you *==access a global variable, Python has to perform a hash table lookup, which involves calculating a hash and then retrieving the value associated with it==*. Though this is optimized, it's still inherently slower than an index-based lookup.


## References:

1. [Why does Python Code Run Faster in a Function?](https://stackabuse.com/why-does-python-code-run-faster-in-a-function/)