---
title: "Python object references"
---

Python uses *reference counting* to inventory objects currently in memory.

It is common for an object to have more than one reference. Under normal
circumstances, the sequence of statements `x = 1; y = 1` does not create two
different objects, each representing the integer value 1. Rather, the first
statement creates a new object representing the integer value 1 *if* such an
object does not yet exist in memory; otherwise, it creates a new reference to
that already existing object: basically, an alias. The second statement also
merely creates a new reference to the already existing object.

In each case, the value of a variable used to track the *reference count* for
the object representing the integer value 1 is incremented. This reference
count will grow as the integer value 1 is assigned to different variables,
passed as arguments (when copied by value), or created in any other context
(such as the initialization of a sequence), and it will shrink as references
go out of scope or variables are reassigned to other objects (or when a
references is removed manually using the `del` statement, or the `remove`
method of a container object). When its reference count declines to zero, it
becomes a possible candidate for garbage collection (that is, reclamation of
memory).


## References

Chun, Wesley J. *Core Python Programming.* 2nd ed, Prentice Hall, 2007. Sections 3.5.4 and 3.5.5, "Reference Counting" and "Garbage Collection"
