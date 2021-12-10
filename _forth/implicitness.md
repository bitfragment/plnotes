---
title: 'Forth implicitness'
---

## Contents
{:.no_toc}

* TOC
{:toc}

## Sources

Leo Brodie, *[Thinking Forth]: A Language and Philosophy for Solving 
Problems,* 2004 Edition, Chapter 1: The Philosophy of Forth

[Thinking Forth]: http://thinking-forth.sourceforge.net/

## Forms

Implicitness is a core characteristic of Forth as a *word-based*
environment. Implicitness in Forth takes two forms: implicit calls and
implicit data passing.

### Implicit calls

Constants, variables, system functions and user-defined commands or
data structures are all called implicitly by name. No explicit call is
necessary.

### Implicit data passing

> The mechanism that produces this effect is Forth's data stack.
> Forth automatically pushes numbers onto the stack; words that require
> numnbers as input automatically pop them off the stack; words that
> produce numbers as output automatically push them onto the stack. The
> words `PUSH` and `POP` do not exist in high-level Forth [...] Forth 
> eliminates the act of passing data from our code, leaving us to
> concentrate on the functional steps of the data's transformation.
—Brodie, *Thinking Forth* 19-20

Here, `1` and `1` are automatically pushed onto the stack; `+` pops
two numbers off the stack and performs addition, pushing the result
onto the stack; and `.` pops the stack to display the result.

```forth
1 1 + .
```

## Execute this file

```sh
$ codedown forth < implicitness.md | gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
1 1 + . 2  ok
```
