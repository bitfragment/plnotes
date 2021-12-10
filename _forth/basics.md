---
title: 'Forth basics'
---

## Contents
{:.no_toc}

* TOC
{:toc}


## Sources

* Leo Brodie, *[Starting FORTH],* Chapter 1: Fundamental Forth
* [Gforth Manual] — [5.24 Programming Tools]

[Gforth Manual]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/
[5.24 Programming Tools]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/Programming-Tools.html#Programming-Tools
[Starting FORTH]: https://www.forth.com/starting-forth/


## Install language

```sh
$ brew install gforth
```

## Use interactively
 
```sh
$ gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
2 2 + . 4  ok
bye
```

## Run file from command line

```sh
$ gforth basics.forth
```

## A first program

A series of word definitions (function definitions) forming a program
that displays a graphical letter 'F' formed using ASCII characters.

```forth
: star      42 emit ;
: stars     0 do star loop ;
: margin    cr 3 spaces ;
: blip      margin star ;
: bar       margin 5 stars ;
: f         bar blip bar blip blip cr ;
f
```

## Display a divider

```forth
: div cr 68 stars ;
div
```

## Other basics

Display a char by specifying literal. Use only inside a word definition.

```forth
: amp [char] & emit ;
amp
```

Specify a string using "dot-quote" format.

```forth
: hello ." Hello world " ;
hello
```

```forth
div
```

## The stack

LIFO (last in, first out).

Push 1 onto the stack. 

```forth
1
```

Inspect the stack non-destructively.

```forth
.s
```

Now pop 1 off the stack and display it.

```forth
.
```

Push 1 and 2 onto the stack. Then pop 2 off the stack and display it.

```forth
1 2 .
```

Test the number at the top of the stack. This pops the stack and pushes
the result of the test, -1 (true), onto the stack

```forth
1 =
```

Pop -1 off the stack and display it.

```forth
.
```

At this point the stack is empty. Trigger stack underflow by trying
to pop it again. This will halt the program, so we comment it out here.

```forth
\ .
```

Catch the 'Address alignment exception' so the program will not halt.

```forth
' . catch
```

```forth
div
```

Clear the stack.

```forth
clearstack
```

## Stack arithmetic

Use postfix notation.

Push 2 onto the stack. Then push 2 onto the stack. `+` takes the top
two numbers off the stack, performs addition, and pushes result onto
the stack.

```forth
2 2 + .
```

This word definition expects a number to be on the stack. It pushes
the number 4 onto the stack, then takes the top two numbers off the
stack, performs addition, and pushes result onto the stack.

```forth
: add4 4 + ;
1 add4 .
```


## Stack-effect comments in word definitions

Format: `( before -- after )`, with "before" meaning what is at the top
of the stack before the call, and "after" meaning what is at the top of
the stack after the call.

```forth
: add5 ( n -- n ) 5 + ;
1 add5 .
```


## Debugging

The word `~~` will print the source line and stack contents at that point.

```forth
: f2 ( -- n ) clearstack 1 ~~ 2 ~~ ;
f2
```


## Gforth assertions

Can only be used in compile-time (colon) definitions. Here, define a 
word `f` that adds one to the number at the top of the stack. Then,
define a test word `f-test` that asserts something about the value
pushed onto the stack by `f`.

```forth
: f3 ( n -- n ) 1 + ;
: f3-test 2 f3 assert( 3 = ) ;
f3-test
```


## Execute this file

```txt
$ codedown forth < basics.md | grep . | gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
: star      42 emit ;  ok
: stars     0 do star loop ;  ok
: margin    cr 3 spaces ;  ok
: blip      margin star ;  ok
: bar       margin 5 stars ;  ok
: f         bar blip bar blip blip cr ;  ok
f
   *****
   *
   *****
   *
   *
 ok
: div cr 68 stars ;  ok
div
******************************************************************** ok
: amp [char] & emit ;  ok
amp & ok
: hello ." Hello world " ;  ok
hello Hello world  ok
div
******************************************************************** ok
1  ok
.s <1> 1  ok
. 1  ok
1 2 . 2  ok
1 =  ok
. -1  ok
\ .  ok
' . catch  ok
div
******************************************************************** ok
clearstack  ok
2 2 + . 4  ok
: add4 4 + ;  ok
1 add4 . 5  ok
: add5 ( n -- n ) 5 + ;  ok
1 add5 . 6  ok
: f2 ( -- n ) clearstack 1 ~~ 2 ~~ ;  ok
f2
*somewhere*:30:<1> 1

*somewhere*:30:<2> 1 2
 ok
: f3 ( n -- n ) 1 + ;  ok
: f3-test 2 f3 assert( 3 = ) ;  ok
f3-test  ok
```