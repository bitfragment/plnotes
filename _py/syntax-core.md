---
title: "Python syntax: core syntax"
---

I've highlighted here only those elements of Python syntax that someone
already familiar with other languages (especially C-like languages) might
want to know.

Code blocks are indented rather than marked with braces. A code block is
called a *suite*. A *clause* contains a *header line* followed by a colon,
then a suite of one or more indented lines.

Python line comments are demarcated by the `#` symbol. Block comments are
formed using triple (single or double)  quotation marks as opening and closing
delimiters. Python enables Lisp-style "documentation strings" in modules,
classes, or  functions, which are accessible at runtime:

```py
def f():
    "Hi, I'm a doc string."
    return True
```

Strings can be either single or double-quoted.

A semicolon separates statements on the same line.

Lines are continued using a backslash (`\`). Within enclosing operators
(parentheses, brackets, braces) or between opening and closing triple quote
marks, the backslash character is not necessary.

In Python 3, `//` is used for floor division. `**` provides  exponentiation.
Standard bitwise operators are available.

`3 < 4 < 5` is a valid expression and the equivalent  of `3 < 4 and 4 < 5`.

Assignments are not expressions, as they are in C, so you cannot obtain a
return value from an assignment.

Python supports augmented assignment (`x += 1`) but not C-style increment or
decrement operators (`x++`, `--x`).

Multiple assignment is easy:

```
x = y = 1               # x = 1; y = 1
(x, y, z) = (1, 2, 3)   # x = 1; y = 2; z = 3
```

The most basic way to display program output and take program input from the
user are `print()` and `input()` respectively.

A **star expression** will "unpack" an iterable of arbitrary length into a list. You can use it in assignment: `(a, *b) = (1, 2, 3)`; in a `for` loop: `for first, *rest in ((1, 2), (3, 4, 5))...`
