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
* [Gforth Manual] — [5.24 Programming Tools]

[Gforth Manual]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/
[5.24 Programming Tools]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/Programming-Tools.html#Programming-Tools
[Starting FORTH]: https://www.forth.com/starting-forth/

## Design

LIFO (last in, first out).

Rightmost item is topmost item. Here, push 1, 2, and 3 onto the data 
stack, then pop and display the top item, 3, using `.`

```forth
1 2 3 . 3  ok
```

## The data stack

### Inspecting

Inspect the data stack non-destructively with `.s`.

```forth
1 2 3 .s <3> 1 2 3  ok
```

### Clearing

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
