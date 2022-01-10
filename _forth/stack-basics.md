---
title: 'Forth stack basics'
---

## Contents
{:.no_toc}

* TOC
{:toc}


## Sources

* Leo Brodie, *[Starting FORTH],* 
  * Chapter 1: Fundamental Forth
  * Chapter 2: How to Get Results
* Edward K. Conklin and Elizabeth D. Rather, *[Forth Programmer's
  Handbook]* 3rd edition, Chapter 1 Introduction
* [Gforth Manual] — [5.24 Programming Tools]

[Gforth Manual]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/
[5.24 Programming Tools]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/Programming-Tools.html#Programming-Tools
[Forth Programmer's Handbook]: https://www.forth.com/product/forth-programmers-handbook/
[Starting FORTH]: https://www.forth.com/starting-forth/

## The data stack

> Every Forth system contains at least one *data stack*. [...] The stack
> is a cell-wide, push-down LIFO (*last in, first out*) list; its
> purpose is to contain numeric operands for Forth commands. Commands
> commonly expect their input parameters on this stack and leave their
> output results there [...] A number encountered by the text
> interpreter will be converted to binary and pushed ontp the stack.
> Forth *data objects* (such as those defined by `VARIABLE` and
> `CONSTANT`) push their addresses or values onto the stack. Thus, the
> stack provides a medium of communication not only between routines by
> between a person and the computer. (Conklin and Rather, *Forth
> Programmer's Handbook* 22)

In reading Forth, the rightmost item is the topmost item on the stack.
Here, push 1, 2, and 3 onto the data stack, then pop and display the top
item, 3, using `.`

```forth
1 2 3 . 3  ok
```

> The push-down stack simplifies the internal structure of Forth and
> produces naturally re-entrant routines. Passing parameters via the
> stack means fewer variables must be named, reducing the amount of
> memory required for named variables. (Conklin and Rather, *Forth
> Programmer's Handbook* 22)

## The return stack

The return stack is used for system functions, to hold return addresses
for nested definitions, and loop parameters. The only system commands
for manipulating the return stack are those for moving parameters
between the data stack and return stack (Conklin and Rather, *Forth
Programmer's Handbook* 25).

### Inspecting the data stack

Inspect the data stack non-destructively with `.s`.

```forth
1 2 3 .s <3> 1 2 3  ok
```

### Clearing the data stack

Clear the data stack with `clearstack`.

### Stack underflow

```forth
clearstack  ok
.s <0>  ok
. 
:17: Address alignment exception
>>>.<<<
Backtrace:
$104EB8728 dup 
$104EB9E50 s>d
```

### Other

Test the item at the top of the data stack. Here, `=` compares 1 with the
topmost item on the stack and pushes the result of the test onto the 
stack: -1 for true, 0 for false.

```forth
1  ok
1 =   ok
.s <1> -1  ok
```
