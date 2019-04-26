---
title: "Python execution"
---

When a Python script is run, it is compiled into platform-independent, but Python-specific *byte code* stored in `*.pyc` files if the program has write access, or generated in memory and later discarded, if not. Python can run a `*.pyc` file even if its source code is not present.

Byte-compiled code is executed by a runtime engine, the Python Virtual Machine, which isn't necessarily a stand-alone program as much as a "big loop that iterates through your byte code instructions, one by one" (Lutz, *Learning Python*).

Python *interpretation* thus consists of these two main steps, *byte-compilation* and *execution.* Unlike in traditional compiled languages like C and C++, there is no separation of compile time and runtime: "the compiler is always present at runtime, and is part of the system that runs programs" (Lutz, *Learning Python*). The built-in functions `eval()` and `exec()` can be passed strings to be interpreted as Python code.


## References

Lutz, Mark. *Learning Python.* 3rd ed, O’Reilly, 2008. Chapter 2: "How Python Runs Programs"
