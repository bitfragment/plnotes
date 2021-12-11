---
title: 'Forth binding'
---

## Contents
{:.no_toc}

* TOC
{:toc}

## Sources

Leo Brodie, *[Thinking Forth]: A Language and Philosophy for Solving 
Problems,* 2004 Edition, Chapter 1: The Philosophy of Forth

[Thinking Forth]: http://thinking-forth.sourceforge.net/

## Variables

A variable name "has but one function: to put on the stack the *address*
of the memory location" where the value is stored (Brodie, *Thinking
Forth* 25). Thus, memory addresses can be passed on the stack.

*Example:* define a variable; store a number in it; display the stored
number; increment the stored number and display it again; test the
stored number; store a different number in the variable; test the new
stored number.

```forth
variable apples
20 apples !
apples ?
1 apples +!
apples ?
21 apples @ = .
40 apples !
40 apples @ = .
```

## Execute this file

```txt
$ codedown forth < binding.md | gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
variable apples  ok
20 apples !  ok
apples ? 20  ok
1 apples +!  ok
apples ? 21  ok
21 apples @ = . -1  ok
40 apples !  ok
40 apples @ = . -1  ok
```
