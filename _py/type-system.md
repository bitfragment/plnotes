---
title: 'Python type system'
---

Python is *dynamically* and *strongly* typed: types are inferred at runtime, but many operations are restricted to objects of a certain type.

What we're accustomed to thinking of as a *variable* name gets created when it
is first assigned a value. To assign a new value to a name is not to "update"
that value; it is to create a new object representing the new value and to
reassign the reference to the name. If you want to delete a reference to an
object, you need to use `del` to do it. A variable in an expression gets replaced with the object that it refers to.

In general, types originate from *object-generation expressions* like `"foo"` or `[1, 2, 3]`, which if you typed it in the Python interpreter, would create a new string and list object, respectively.

Types are not associated with names; they are associated with objects. "Variables are generic in nature; they always simply refer to a particular object at a particular point in time" (Lutz, *Learning Python*).

In addition to a *reference counter* (see [Python object reference]({{site.baseurl}}/py/object-reference/)), each object has as a standard header field a *type designator.* Lutz, *Learning Python*:

> The integer object 3, for example, will contain the value 3, plus a designator that tells Python that the object is an integer (strictly speaking, a pointer to an object called int, the name of the integer type). The type designator of the 'spam' string object points to the string type (called str) instead. Because objects know their types, variables don’t have to.



## Sources

Lutz, Mark. *Learning Python.* Third ed., O'Reilly, 2008. Chapter 4, "Introducing Python Object Types"; Chapter 6, "The Dynamic Typing Interlude"
