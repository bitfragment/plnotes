---
title: 'Forth: basics'
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
: star      42 emit ;         \ Display ASCII 42 (asterisk)
: stars     0 do star loop ;  \ Call `star` n times
: margin    cr 3 spaces ;     \ Display carriage return and 3 spaces
: blip      margin star ;
: bar       margin 5 stars ;
: f         bar blip bar blip blip cr ;

f cr
```

Output:

```sh
   *****
   *
   *****
   *
   *
```


## Other basics

Display a char by specifying literal. Use only inside a word definition.

```forth
: star2     [char] & emit ;
star2 cr
```

Specify a string using "dot-quote" format.

```forth
: hello     ." Hello world " ;
hello cr
```


## The stack

LIFO (last in, first out).

Push 1 onto the stack. 

```forth
1         ."       <- push 1 onto stack " cr
```

Inspect the stack non-destructively.

```forth
.s        ." <- inspect stack non-destructively " cr
```

Now pop 1 off the stack and display it.

```forth
.         ."     <- pop stack and display " cr
```

Push 1 and 2 onto the stack. Then pop 2 off the stack and display it.

```forth
1 2 .     ."     <- push 1 and 2 onto stack then pop and display " cr
```

Pop 1 off the stack and display it.

```forth
.         ."     <- pop and display " cr
```

At this point the stack is empty. Trigger stack underflow by trying
to pop it again. This will halt the program, so we comment it out here.

```forth
\ .
```

Catch the 'Address alignment exception' so the program will not halt.

```forth
' . catch ."       <- catch exception " cr
```

## Stack arithmetic

Use postfix notation.

Push 2 onto the stack. Then push 2 onto the stack. `+` takes the top
two numbers off the stack, performs addition, and pushes result onto
the stack.

```forth
." Stack arithmetic: " cr

2 2 + . cr

." 2 + 2 = "
2 2 + . cr
```

This word definition expects a number to be on the stack. It pushes
the number 4 onto the stack, then takes the top two numbers off the
stack, performs addition, and pushes result onto the stack.

```forth
: add4      4 + ;
." 1 + 4 = "
1 add4 . cr
```


## Stack-effect comments in word definitions

Format: `( before -- after )`

```forth
: add5 ( n -- n ) 5 + ; \ before, one number should be on the stack,
                        \ after, one number should be on the stack.

." 1 + 5 = "
1 add5       \ now the result is on the stack
. cr
```


## Debugging tools

The word `~~` will print the source line and stack contents at that point.

```forth
: f ( -- n ) 1 ~~ 2 ~~ + ~~ . ~~ ;
f
```


## Exit

```forth
bye
```


Output:

```sh
&
Hello world 
      <- push 1 onto stack 
<1> 1 <- inspect stack non-destructively 
1     <- pop stack and display 
2     <- add 1 and 2 to stack then pop and display 
1     <- pop and display 
      <- catch exception 
Stack arithmetic:
4 
2 + 2 = 4 
1 + 4 = 5 
1 + 5 = 6 
/Users/blennon/sync/desk/plnotes/_forth/basics.forth:55:<1> 1 
/Users/blennon/sync/desk/plnotes/_forth/basics.forth:55:<2> 1 2 
/Users/blennon/sync/desk/plnotes/_forth/basics.forth:55:<1> 3 
/Users/blennon/sync/desk/plnotes/_forth/basics.forth:55:<0> 
```
