---
title: "Python function arguments"
---

Full syntax for function call:

``` 
func(positional_args,
     keyword_args,
     *tuple_nonkeyword_args,
     **dict_keyword_args)
```

Positional arguments only:

```py
def f(a, b, c):
    return a + b + c
```

If called as keyword arguments, position is irrelevant:

```py
assert f(c=1, a=1, b=1) == 3
```

If mixing positional and keyword arguments, positional arguments must come
first:

```py
assert f(1, 1, c=1) == 3
```

Specifying a default argument also makes the argument optional. No `TypeError`
will be thrown.

```py
def f(a, b, c=1):
    return a + b + c

assert f(1, 1) == 3
```


Variable-length non-keyword arguments: all non-keyword arguments beginning at
the position of `*b`  will be gathered into a tuple.

```py
def f(a, *b):
    return a + sum(b)

assert f(1, 1, 1) == 3
```

Variable-length keyword arguments: all keyword arguments beginning at the the
position of `**b` will be gathered into a dictionary.

```py
def f(a, **b):
    return a + sum(b.values())
    
assert f(1, b1=1, b2=1) == 3
```


In combinations, positional arguments, keyword arguments, variable-length non-
keyword arguments, and variable- length keyword arguments must appear in that
order.

```py
def f(a, b=1, *c, **d):
    return a + b + sum(c) + sum(d.values())

assert f(1, b=1, c1=1, c2=1) == 4
```


You can't pass tuple or dictionary objects as arguments without destructuring
them:

```py
b = (1, 1)
c = {'c1': 1, 'c2': 2}
assert f(1, *b, **c) == 6
```
