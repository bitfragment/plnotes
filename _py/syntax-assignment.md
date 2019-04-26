---
title: "Python syntax: assignment"
---

You can assign the elements of **any sequence or iterable** to discrete names,
provided that the number of elements on the left-hand side of the assignment
expression is equal to the number of elements on its right-hand side. Python
calls this procedure "unpacking":

```py
>>> x, y = 1, 2
>>> x; y
1
2
>>> (a, b, c) = (1, 2, (3, 4, 5))
>>> a; b; c
1
2
(3, 4, 5)
```

By convention, `_` is used for values to be discarded. E.g. `s, _ = "foo", "bar"`.

Use a **[star expression]({{site.baseurl}}/py/syntax-star-expression/)** to
unpack an arbitrary number of elements: `(a, b, *c) = (1, 2, 3, 4, 5)`


## References

Beazley, David, and Brian K. Jones. *Python Cookbook: Recipes for Mastering Python 3.* 3. ed, O’Reilly, 2013.
