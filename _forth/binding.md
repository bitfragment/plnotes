---
title: 'Forth binding'
---

## Contents
{:.no_toc}

* TOC
{:toc}

## Sources

* Leo Brodie, *[Starting Forth]: An Introduction to the FORTH Language
  and Operating System for Beginners and Professionals,* online edition,
  Chapter 3: The Editor (and Staff)
* Leo Brodie, *[Thinking Forth]: A Language and Philosophy for Solving 
  Problems,* 2004 Edition, Chapter 1: The Philosophy of Forth
* Edward K. Conklin and Elizabeth D. Rather, *[Forth Programmer's
  Handbook]* 3rd edition, Chapter 1 Introduction

[Forth Programmer's Handbook]: https://www.forth.com/product/forth-programmers-handbook/
[Starting Forth]: https://www.forth.com/starting-forth/
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

## Word definitions

> Forth allows any kind of ASCII string (except one containing spaces)
> to be a valid name, and this introduces some ambiguities [...] For
> instance, Fort calls subroutines *words*, but *word* could also mean
> an addressable unit of memory. To resolve this, we use the following
> conventions:
> * A Forth execution procedure is called a *definition*. A *word* is
>   the name of such a definition.
> * The word length of the processor is always referred to as a *cell*.
>   This is also the size of an address and the size of a single item on
>   Forth's stacks.
>
> (Conklin and Rather, *Forth Programmer's Handbook 18)

*Colon definitions* are compiled, not interpreted like other phrases
in the environment.

Format: `: <name> <words to be executed> ;`

```forth
: f ( n -- n ) 1 + ;
1 f 2 = 0= s" test failed " exception and throw
```

Words can also be defined in assembler code, or function as data objects
instead of procedures. But

> [r]egardless of the kind of definition it is, each word is basically
> the same: an executable function with a defined behavior. This is true
> even of things that seem analogous to data objects in other languages
> and things that look like punctuation. There is no punctuation in
> Forth, and no syntax, just executable words. (Conklin and Rather,
> *Forth Programmer's Handbook 19)

You can redefine a word using a colon definition. Only the most recent
definition will be executed, but earlier definitions will be retained
in the dictionary.

Early versions of Forth had a `forget` word that could be used to remove
any most recent word definition from the dictionary. This is now an
obsolete feature. The word `marker` with a null definition  allows for
restoring the system state to an earlier moment, with all words defined
after that point being removed from the dictionary.

```forth
marker -restore
: f ( n -- n ) 2 + ;
1 f 3 = 0= s" test failed " exception and throw

-restore
1 f 2 = 0= s" test failed " exception and throw
```

## Execute this file

```txt
$ codedown forth < binding.md | grep . | gforth
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
: f ( n -- n ) 1 + ;  ok
1 f 2 = 0= s" test failed " exception and throw  ok
marker -restore  ok
: f ( n -- n ) 2 + ; redefined f   ok
1 f 3 = 0= s" test failed " exception and throw  ok
-restore  ok
1 f 2 = 0= s" test failed " exception and throw  ok
```
