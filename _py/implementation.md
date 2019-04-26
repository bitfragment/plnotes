---
title: "Python implementation"
---

Python is a **byte-compiled** language, like Java. Byte-compiled code is executed on the PVM (Python Virtual Machine). Byte compilation provides portability at the expense of execution speed in some contexts.

Python is implemented in different ways. The Python interpreter might be implemented as an executable program on one system (for example, as a C program), or a set of libraries on another (for example, as a set of Java classes).


## CPython (C)

The standard (reference) implementation is CPython, written in portable ANSI C. System tasks like processing a file are executed by compiled C code, at the speed of C. Each Python function call produces a C function call, so CPython is limited by the C stack, which can make problems for recursion. A stackless implementation of CPython also exists.


## Jython (Java)

Jython is a Python interpreter in Java, which can run Python code on the JVM and provides access to Java class libraries and other inter-operability. It is implemented as a set of Java classes that compile Python source to Java byte code for execution on the JVM. Just as CPython can control (script) C/C++ components, Jython can control Java components. In Jython one can import and use Java classes natively.


## IronPython (C\#)

IronPython is a C\# implementation for .Net and Mono.


## References

Chun, Wesley J. *Core Python Programming.* 2nd ed, Prentice Hall, 2007. Chapter 1: "Welcome to Python!"

Lutz, Mark. *Learning Python.* 3rd ed, O’Reilly, 2008. Chapter 1: "A Python Q&A Session" and Chapter 2, "How Python Runs Programs"
