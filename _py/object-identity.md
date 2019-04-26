---
title: "Python object identity"
---

**Identity** is one of three characteristics (along with **type** and
**value**) assigned to a Python object when it is created. It is represented
by a read-only integer value identifier, guaranteed to be unique during the
object's lifetime (outside of which it may be reused). That identifier is
returned by the built-in function [`id()`]. "This value," Wesley J. Chun
suggests, "is as close as you will get to a 'memory address' in Python." (That
is how it is implemented in CPython.)

[`id()`]: https://docs.python.org/3/library/functions.html#id

While `id()` does not have much everyday use, it offers a handy and easy way to 
understand how Python uses objects.

```py
>>> a = 1
>>> id(a)
4327371344
```

The identity reported here is the identity of the object referred to by the
name `a`. In this example, that is an object representing the integer value 1.
If we assign the object referred to by `a` to a second name, `b`, we have
created a second reference to the same object.

```py
>>> a = 1
>>> id(a)
4339520080
>>> b = a
>>> id(b)
4339520080
```

Note that in the following example, `id()`​ will report that same identity
when called on (1) the integer literal, as a represented value; (2) the first
reference to it; (3) subsequent separate references to it, of three different
kinds. Assuming a fresh environment:

```py
>>> id(1)
4402483792
>>> a = 1
>>> id(a)
4402483792
>>> b = 1
>>> id(b)
4402483792
>>> c = b
>>> id(1) == id(a) == id(b) == id(c)
True
>>> for _ in range(1, 2):
    id(_)
4402483792
```

The reason for this is — though note, this is an implementation-dependent
feature — that Python caches, or "interns," integers in the range -5 to 256,
which are likely to be used many times in a typical program. (For an
explanation including a walk through the relevant CPython source code, see
"['is' operator behaves unexpectedly with integers].") Because this feature may
be present, and also because it is implementation-dependent, you should never
use the `is` operator to compare integers. The value returned by `id()` is a
memory address in CPython, but `id()` might be implemented differently in
Python implemented on other platforms.


## Boolean values

Booleans also behave this way:

```py
>>> a = True
>>> b = True
>>> id(True) == id(a) == id(b)
True
```

## Floating-point values

Since Python does not cache floating-point values in this way, equivalent
floating-point literals within one expression will refer to the same object,
but assignment in discrete expressions will create different objects:

```py
>>> id(1.1) == id(1.1)
True
>>> a = 1.1
>>> b = 1.1
>>> id(a) == id(b)
False
>>> id(1.1) == id(a)
False
>>> id(1.1) == id(b)
False
```

That is, as long as you're using the Python interpreter and you're using
separate, thus separately parsed expressions. Run them together on a line,
giving the bytecode compiler a chance to optimize by using the same object
for equivalent literals, and you'll get back a single object:

```py
>>> a = 1.1; b = 1.1; id(a) == id(b)
True
```


## Sequences and collections 

Sequences and collections are yet another case. Equivalent literals within
one expression will refer to the same object, but that's the only case.

```py
>>> id([1, 2, 3]) == id([1, 2, 3])
True
>>> a = [1, 2, 3]
>>> b = [1, 2, 3]
>>> id(a) == id(b)
False
>>> id([1, 2, 3]) == id(a)
False
>>> a = [1, 2, 3]; b = [1, 2, 3]; id(a) == id(b)
False
>>> id((1, 2, 3)) == id((1, 2, 3))
False
>>> a = (1, 2, 3)
>>> b = (1, 2, 3)
>>> id(a) == id(b)
False
>>> id((1, 2, 3)) == id(a)
False
>>> a = (1, 2, 3); b = (1, 2, 3); id(a) == id(b)
False
>>> id({'a': 1}) == id({'a': 1})
True
>>> a = {'a': 1}
>>> b = {'a': 1}
>>> id(a) == id(b)
False
>>> id({'a': 1}) == id(a)
False
>>> a = {'a': 1}; b = {'a': 1}; id(a) == id(b)
False
```

However, literals within the sequence or collection will refer to the
same object:

```py
>>> a = [1, 2, 3]
>>> b = [1, 2, 3]
>>> id(a[0]) == id(b[0])
True
>>> a = (1, 2, 3)
>>> b = (1, 2, 3)
>>> id(a[1]) == id(b[1])
True
>>> a = {'a': 1}
>>> b = {'a': 1}
>>> id(a['a']) == id(b['a'])
True
```


## Strings

Finally, strings are yet another case. Python also interns valid names — that
is, symbols with an initial alphabetic character or underscore, followed by
additional characters, if any, that are alphanumeric or underscore:

```py
>>> a = "foo"
>>> b = "foo"
>>> id("foo") == id(a) == id(b)
True
>>> a = "foo bar"
>>> b = "foo bar"
>>> id(a) == id(b)
False
```


## Sources

Chun, Wesley J. *Core Python Programming.* 2nd ed, Prentice Hall, 2007. Section 4.1: "Python Objects."

felix021. "[What's with the Integer Cache inside Python?]" Stack Overflow,
March 2, 2013.

Hewgill, Greg. "['is' operator behaves unexpectedly with integers]." Stack
Overflow, November 20, 2008.

Lutz, Mark. *Learning Python.* Third ed., O'Reilly, 2008. Chapter 6, "The Dynamic Typing Interlude."

Vernier, Yann. "[The internals of Python string interning]." Guilload.com, June 28, 2014.

"[PyObject* PyLong_FromLong(long v)]." Python/C API Reference Manual: Integer Objects.

['is' operator behaves unexpectedly with integers]: https://stackoverflow.com/questions/306313/is-operator-behaves-unexpectedly-with-integers

[PyObject* PyLong_FromLong(long v)]: https://docs.python.org/3/c-api/long.html#c.PyLong_FromLong

[The internals of Python string interning]: http://guilload.com/python-string-interning/

[What's with the Integer Cache inside Python?]: https://stackoverflow.com/questions/15171695/whats-with-the-integer-cache-inside-python
