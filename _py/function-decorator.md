---
title: "Python function decorators"
---

Decorator function that takes a function as argument and does something with
it. Use case: wrapping.

```py
def d1(f):
    x = 1
    f(x)
```

When the decorator syntax is used, the decorator function is called.

```py
@d1
def f(x):
    print("d1() called")
    assert x == 1
```

Decorator function that takes an argument and returns a decorator.

```py
def d2(x):
    def f(g):
        assert g() == x
    return f


@d2(1)
def f():
    print("d2() called")
    return 1
```
