---
title: "Python object model"
---

## A useful generalization

Start with a useful generalization: in Python, "everything" is an object.

It's a useful generalization because it holds at some distance the more
fundamental image of contiguous and/or contiguously addressed regions of
memory, to which primitive representations of primitive data values might be
written more or less sequentially. That is, it holds at some distance the
analogy to other kinds of writing (writing an article or report, a letter or
message, or a novel) which still fundamentally determines how we talk about
programming.[^eng]

[^eng]: At least in the English language.

It's not that the oft-remarked ease of writing Python code, attributed to both
its unencumbered syntax and its resemblance to English-language pseudocode,
doesn't encourage the experience of a flow state directly analogous to other
kinds of writing. Certainly it does. (Some might even say that Python does that
to a fault.)

Take the specific, yet (still) paradigmatic object-oriented model offered by
Java. I'd wager that the idea of object orientation is easier for most of us
to grasp by appeal to primarily spatialized images of "building blocks" or
other integral objects at rest, statically arranged in a spatial field within
which they communicate.

Java's object orientation redressed the shortcomings of a procedural model in
which data flows freely through functions, rather than being bundled
(encapsulated) with functions, granted "privacy," and otherwise protected from
outside interference. The procedural model is easier to assimilate to our
primarily temporal understanding of writing as an ongoing process that, even
as it describes, defines and clarifies, also renders static objects dynamic
and integral objects unstable in their objecthood. If you've ever felt that
there's something faintly inappropriate in the description of programming in
Java as "writing a program," and that "building a program" more effectively
describes that process, that's what I mean. Though Java's C-like imperative
syntax constrains the programmer to a linear style of procedural instruction,
in Java that temporal linearization sits in tension with the spatialized
static domain of the objects without which one can get literally nothing done.

Let's say that Python, in which everything is an object (whereas in Java
everything is a class) presents a different case of that tension, in line with
what Mark Pilgrim calls Python's comparatively "loose" concept of objects.


## What's "everything"?

From the perspective of a Python user, "everything" means any representation
of a primitive value and any data structure one might work with in Python,
from a small integer value all the way to a collection of class instances.

Understanding this requires one to combine a Python user's perspective with a
language designer's perspective. When we say that "everything" in Python is an
object, we mean that both apparently primitive values (for example, a small
integer value) and relatively complex values like character string literals
are conceptualized and implemented as encapsulations of data. So are Python's
other built-in sequences and collections — lists, tuples, and dictionaries. So
are functions. So are modules.

Perhaps most difficult to grasp, if you are used to the conventional
conceptual distinction between class and "object" used to mean class
instance, is that Python classes themselves, as well as class instances, are
objects:

```py
>> class C:
       a = 1
>>> X = C     # assign declared class itself to new identifier `X`
>>> y = X()   # instantiate via new identifier `X`
>>> y.a
1
```

This would be nonsensical in the paradigm established by Java, in which a
class serves as a template used to produce a new, not-yet-existing object,
thus *instantiating* the template. It's possible not only because in Python,
one does not declare variables and assign them values in the conventional
sense: that is, by binding an identifier to the address of a segment(s) of
memory allocated specifically for an item of a specified data type, then
writing data beginning at that address, from which it may subsequently be
read. Rather, assignment in Python binds a named reference (implemented as a
pointer) to an object, and the allocation of memory is for the object, not the
data type declared for the identifier.

Python is dynamically types, so no data types (`char`, `int`, `float`, etc.)
are declared for identifiers in Python. This is one reason why a named
reference can always be assigned another object:

```py
>>> C = "foo"
>>> C
'foo'
```


## Object types are type objects

As another illustration of what it means to say that "everything" is an object
in Python, consider the return value of the Python built-in function
[`type()`](https://docs.python.org/3/library/functions.html#type).

When passed the reference represented by the original class `C` as an
argument, [`type()`](https://docs.python.org/3/library/functions.html#type)
returns the appropriate type information, `<class 'type'>`. When the reference
represented by `C` is reassigned to point to the string object represented as
"foo", the type information returned by `type()` is `<class 'str'>`.

Note that these aren't string literals: they're representations of objects, which
happen to contain (identifying) strings as attributes. The units of type
information (that is, the object types) `<class 'type'>` and `<class 'str'>`
are themselves (type) objects.


## References

Chun, Wesley J. *Core Python Programming.* 2nd ed, Prentice Hall, 2007.
Section 4.3.1 "Type Objects and the `type` Type Object."

Pilgrim, Mark. *Dive Into Python 3*. Apress, 2009.
Section 1.5 "[Everything Is an Object](http://www.diveintopython3.net/your-first-python-program.html#everythingisanobject)."


## Notes
