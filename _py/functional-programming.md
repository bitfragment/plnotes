---
title: "Python functional programming"
---

## Contents
{:.no_toc}

* TOC
{:toc}


## Functions are first-class citizens

```py
def f():
    return 1


g = f
assert g() == 1

x = {"f1": f, "f2": g}
y = x["f1"]
assert y() == 1
```


## Function composition

```py
def f(x):
    return x() + 1


def g():
    return 1


assert f(g) == 2
```

## Returning a function

```py
def make_one_adder(x):
    def f():
        return x + 1

    return f


my_one_adder = make_one_adder(1)
assert my_one_adder() == 2

my_one_adder = make_one_adder(2)
assert my_one_adder() == 3


def make_adder(x):
    def f(y):
        return x + y

    return f


my_two_adder = make_adder(2)
assert my_two_adder(1) == 3

my_three_adder = make_adder(3)
assert my_three_adder(1) == 4


def make_prepender(x):
    def f(y):
        return x + "-" + y

    return f


my_foo_prepender = make_prepender("foo")
assert my_foo_prepender("abc") == "foo-abc"

my_bar_prepender = make_prepender("bar")
assert my_bar_prepender("abc") == "bar-abc"
```


## Lambda functions

Not considered Pythonic.

```py
assert callable(lambda x: x + 1)

assert (lambda x: x + 1)(1) == 2

f = lambda x: x + 1
assert f(1) == 2

data = ['foo', 'bar', 'baz']
```

Sort it by the second character:

```py
assert sorted(data, key=lambda x: x[1]) == ['bar', 'baz', 'foo']
```

## `map()`, `filter()`, `reduce()`

The first two are still in the standard library. `reduce()` was banished to `functools`.

Not considered Pythonic. Use list comprehensions instead.

"Guido [van Rossum] actually advocated for eliminating all three of `reduce()`, `map()`, and `filter()` from Python. [...] As it happens, the previously mentioned list comprehension covers the functionality provided by all these functions and much more."

All three return iterables.

```py
data = list(map(lambda x: 'qux' + x, data))
assert data == ['quxfoo', 'quxbar', 'quxbaz']

data = list(filter(lambda x: 'ba' in x, data))
assert data == ['quxbar', 'quxbaz']

from functools import reduce
assert reduce(lambda x, y: x + y, data) == 'quxbarquxbaz'
```

`reduce()` including an initial value:

```py
assert reduce(lambda x, y: x + y, data, 'foo') == 'fooquxbarquxbaz'
```

A more obvious use case:

```py
assert reduce(lambda x, y: x + y, [1, 2, 3], 1) == 7
```

`map()` will accept multiple iterables:

```py
data = (('a', 'a', 'a'), ('b', 'b', 'b'), ('c', 'c', 'c'))

data = list(map(lambda x, y, z: x + y + z, *data))
assert data == ['abc', 'abc', 'abc']
```

`map()` and `filter()` can be implemented using `reduce()`:

```py
def _map(func, iterable):
    return reduce(
        lambda items, val: items + [func(val)],
        iterable,
        []
    )    

data = _map(lambda x: 'foo' + x, data)
assert data == ['fooabc', 'fooabc', 'fooabc']


def _filter(func, iterable):
    return reduce(
        lambda items, val: items + [val] if func(val) else items,
        iterable,
        []
    )

data[-1] = 'abcfoo'
data = _filter(lambda x: x[0] != 'a', data)
assert data == ['fooabc', 'fooabc']
```
