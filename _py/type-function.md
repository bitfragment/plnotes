---
title: "Python type functions"
---

`type()` returns a **type object**: `type(1) ⟹ <class 'int'>`

`isinstance()`, which you should use when introspecting an object's type,
returns a Boolean: `isinstance(1, int) ⟹ True`

`cmp()`, which calls the class's `__cmp__()` method, compares two objects.
Like C `strcmp()`, it returns a negative number if object 1 is less than
object 2, zero if object 1 is equal to object 2, and a positive number if
object 1 is greater than object 2.

`str()`, which calls the class's `__str__()` method, returns a human-readable
and printable string representation of an object.

`repr()`, which calls the
class's `__repr__()` method,  returns a (usually) *evaluatable* string
representation of an object, one that can be passed to `eval()`.


## `str()` and `repr()`

Here, `eval(str(s))` evaluates the string "foo" treating it as a name, while
`eval(repr(s))` evaluates the string "foo" treating it as a string:

```py
>>> s = "foo"
>>> str(s)
'foo'
>>> repr(s)
"'foo'"
>>> eval(str(s))
Traceback (most recent call last):
  File "<pyshell#25>", line 1, in <module>
    eval(str(s))
  File "<string>", line 1, in <module>
NameError: name 'foo' is not defined
>>> eval(repr(s))
'foo'
```

Here, `eval(str(s))` evaluates the string "1 + 1" treating it as an
expression, while `eval(repr(s))` evaluates the string treating it as a
string:

```py
>>> s = "1 + 1"
>>> eval(str(s))
2
>>> eval(repr(s))
'1 + 1'
```


## References

Chun, Wesley J. *Core Python Programming.* 2nd ed, Prentice Hall, 2007. Section 4.6, "Standard Type Built-in Functions."
