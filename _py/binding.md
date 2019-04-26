---
title: 'Python binding'
---

A *variable name* is not quite that in Python. When you assign a value to a name, the following three steps are performed: (1) a new object is created to represent the value; (2) if the name does not yet exist, it is created; (3) the (possibly new) name is made to *refer* to the new object — that is, a *reference* (in CPython, implemented as a C pointer) is established. 

Lutz, *Learning Python:*

> *Variables* are entries in a system table, with spaces for links to objects. *Objects* are pieces of allocated memory, with enough space to represent the values for which they stand. *References* are automatically followed pointers from variables to objects.

*The name may not always be new; but the object will be new, except where it has been interned,* or cached (see [Python object identity]({{site.baseurl}}/py/object-identity/)). Lutz, *Learning Python:*

> At least conceptually, each time you generate a new value in your script by running an expression, Python creates a new object (i.e., a chunk of memory) to represent that value. Internally, as an optimization, Python caches and reuses certain kinds of unchangeable objects, such as small integers and strings (each 0 is not really a new piece of memory...). But, from a logical perspective, it works as though each expression’s result value is a distinct object, and each object is a distinct piece of memory.


## Rebinding

To assign a new value to a name is not to "update" that value; it is to create
a new object representing the new value and to reassign the reference to the
name. If you want to delete a reference to an object, you need to use `del` to
do it.



## Sources

Lutz, Mark. *Learning Python.* Third ed., O'Reilly, 2008. Chapter 6, "The Dynamic Typing Interlude"
