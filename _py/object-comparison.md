---
title: "Python object comparison"
---

## Object value comparison

**Object values** are values represented by Python objects. The object value
of the object representing the integer value 1 is 1.

For comparing object values, Python provides the C-style comparison operators
`<`, `>`, `<=`, `>=`, `==` and `!=`.  (Python 2 had the ABC/Pascal style `<>`
"not equal to" operator.)

These operators compare numeric values according to (signed) magnitude and
compare strings lexicographically.

Python introduced the `Boolean` type in version 2.3. Up to that point,
comparisons yielded the integer values 1 and 0; beginning with 2.3, they yield
the Boolean values `True` or `False`.

An unusual Python feature is that multiple comparisons can be made on the same line and are evaluated from left to right: `1 < 2 > 1` is equivalent to
`1 < 2 and 2 > 1`.


## Object identity comparison

**Object identities** are the integer values serving as unique identifiers of
Python objects. The object identity of the object representing the integer
value 1 is returned by the expression `id(1)`.

For comparing object identities, Python provides the operators `is` and `is not`.
`is` will return `True` if two references are to the same object:

```py
>>> a = 1
>>> b = a
>>> id(a) == id(b)
True
>>> a is b
True
```


## References

Chun, Wesley J. *Core Python Programming.* 2nd ed, Prentice Hall, 2007. Section 4.5: "Standard Type Operators"
