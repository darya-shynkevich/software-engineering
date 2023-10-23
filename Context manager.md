A context manager is **an object that can be used in a `with` block** to sandwich some code between an entrance action and an exit action.

Context managers need a `__enter__` method and a `__exit__` method, and the `__exit__` method should accept three positional arguments:

```Python
class Example:
    def __enter__(self):
        print("enter")

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("exit")
```

```Python
>>> with Example() as example:
...     print("Yay Python!")
...
enter
Yay Python!
exit
```

The `as` keyword will point a given variable name to **the return value from the `__enter__` method**.

```Python
>>> with Example() as result:
...     print("Result from __enter__ method:", result)
...
Result from __enter__ method: None. # Because __enter__ returns None
```

#### The return value of `__enter__`

```Python
import time

class Timer:
    def __enter__(self):
        self.start = time.perf_counter()
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.stop = time.perf_counter()
        self.elapsed = self.stop - self.start
```

This context manager will time how long it took to run a particular block of code (the block of code in our `with` block).

We can use this context manager by making a `Timer` object, using `with` to run a block of code, and then checking the `elapsed` attribute on our `Timer` object:

```Python
>>> with Timer() as t:
...     result = sum(range(10_000_000))
...
>>> t.elapsed
0.3115791230229661
```

#### The arguments passed to `__exit__`

If an [exception](https://www.pythonmorsels.com/how-to-catch-an-exception-in-python/?watch) occurs within a `with` block, these three arguments passed to the context manager's `__exit__` method will be:

1. the exception class
2. the [exception object](https://www.pythonmorsels.com/what-can-you-do-with-exception-objects/)
3. a traceback object for the exception

But if no exception occurs, those three arguments will all be `None`.

```Python
import logging

class LogException:
    def __init__(self, logger, level=logging.ERROR, suppress=False):
        self.logger, self.level, self.suppress = logger, level, suppress

    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type is not None:
            info = (exc_type, exc_val, exc_tb)
            self.logger.log(self.level, "Exception occurred", exc_info=info)
            return self.suppress
        return False
```

#### **The return value of `__exit__`**

==If the `__exit__` method returns something true or [truthy](https://www.pythonmorsels.com/truthiness/), whatever exception was being raised will actually be suppressed.==

By default, `__exit__` returns `None`, just as every function does by default. If we return `None`, which is falsey, or `False`, or anything that's falsey, `__exit__` **won't do anything different from its default**, which is to just continue raising that exception.

But if `True` or a truthy value is returned, **the exception will be suppressed**.

### Aside: what about `contextmanager`?

```Python
from contextlib import contextmanager
import os


@contextmanager
def set_env_var(var_name, new_value):
    original_value = os.environ.get(var_name)
    os.environ[var_name] = new_value
    try:
        yield
    finally:
        if original_value is None:
            del os.environ[var_name]
        else:
            os.environ[var_name] = original_value
```

### References:

1. [Creating a context manager in Python](https://www.pythonmorsels.com/creating-a-context-manager/)