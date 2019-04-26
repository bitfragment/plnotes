---
title: "Python syntax: star expression"
---

Python's star expression enables the division of sequences and iterables of
arbitrary length in a manner similar to the `first` and `rest` routines of
list processing languages. The star expression will unpack a sequence of
arbitrary length into a list:

```py
>>> (first, *rest) = (1, 2, 3, 4, 5); print(first, rest)
1 [2, 3, 4, 5]
```

You can use it in any position in a sequence of names:

```py
>>> (*a, b, c) = (1, 2, 3, 4, 5); print(a, b, c)
[1, 2, 3] 4 5
>>> (a, *b, c) = (1, 2, 3, 4, 5); print(a, b, c)
1 [2, 3, 4] 5
>>> (a, b, *c) = (1, 2, 3, 4, 5); print(a, b, c)
1 2 [3, 4, 5]
```

What is unpacked by the star expression will always be a list, even if empty:

```py
>>> (a, b, *c) = (1, 2); c
[]
```

Use in a `for` loop over so-called "tagged tuples" of arbitrary length:

```py
>>> xs = ( ('a', 1, 2), ('b', 3, 4, 5), ('c', 6) )
>>> for first, *rest in xs:
    print(first, rest)
a [1, 2]
b [3, 4, 5]
c [6]
```

Use with the conventional representation of discarded elements by `_`:

```py
>>> (first, *_, last) = (1, 2, 3, 4, 5); print(first, _, last)
1 [2, 3, 4] 5
```


## References

Beazley, David, and Brian K. Jones. *Python Cookbook: Recipes for Mastering Python 3.* 3. ed, O’Reilly, 2013.
