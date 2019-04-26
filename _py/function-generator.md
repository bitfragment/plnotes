---
title: "Python generator functions"
---

```py
def f(begin, end):
    while begin <= end:
        yield begin
        begin += 1

g = f(1, 10)
```

Get the next value only with `next()`.

```py
assert next(g) == 1
```

This calls the object's `__next__()` method.

```py
assert g.__next__() == 2
```

Python `for` loops call `next()` and handle the `StopIteration` exception
raised when all values have been yielded.

```py
x = 3
for item in g:
    assert item == x
    x += 1
```

Generator is now exhausted.

```py
assert list(g) == []
```

Enabling use of `send()` to send back a value.

```py
def f(begin, end):
    while begin <= end:
        val = (yield begin)
        if val is not None:
            begin = val
        else:
            begin += 1


g = f(1, 10)
assert next(g) == 1
assert next(g) == 2
assert next(g) == 3

g.send(0) # reset value
assert next(g) == 1
```

Call `close()` to exhaust the generator.

```py
g.close()
assert list(g) == []
```
