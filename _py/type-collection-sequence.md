---
title: 'Python types: collection types: sequences'
---

A sequence type is a left-to-right "positionally ordered collection of other objects" (Lutz, *Learning Python*). 

**Strings** and **tuples** are immutable; **lists** are mutable.

Sequence types are bounds-checked: if you access or assign to an index position that is out of bounds, you'll get an `IndexError`.

Sequence types other than strings can be **arbitrarily nested.**


## Operations common to all sequence types

Sequence types store their **length**, which can be accessed with `len()`. They are **zero-indexed**. Items can be accessed using **subscript notation**, including negative subscript notation (-1 for the last item, -2 for the second to last item, etc.). Arbitrary expressions can be used in subscript notation: e.g., `myseq[1 + 1]`. Subscript notation also supports **slicing**: e.g., `myseq[:3]`, `myseq[3:]`, `myseq[1:3]`. Sequences can be **concatenated** using the overloaded `+` operator, and they can be extended using the overloaded `+=` and `*` operators: `a = [1]; a += [2]` produces `[1, 2`], while `"a" * 3` produces `"aaa"`, `[1] * 3` produces `[1, 1, 1]`, and `(1,) * 3` (using the comma to ensure interpretation as a tuple) produces `(1, 1, 1)`.


## Sources

Lutz, Mark. *Learning Python.* Third ed., O'Reilly, 2008. Chapter 4, "Introducing Python Object Types."
