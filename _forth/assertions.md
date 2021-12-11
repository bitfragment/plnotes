---
title: 'Forth assertions'
---

## Contents
{:.no_toc}

* TOC
{:toc}


## Sources

* [Gforth Manual] — [5.24 Programming Tools]

[Gforth Manual]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/
[5.24 Programming Tools]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/Programming-Tools.html#Programming-Tools

## Gforth assertions

Can only be used in compile-time (colon) definitions. Here, define a 
word `add1` that adds one to the number at the top of the stack. Then,
define a test word `add1-test` that asserts something about the value
pushed onto the stack by `add1`.

```forth
: add1 ( n -- n ) 1 + ;
: add1-test 2 add1 assert( 3 = ) ;
add1-test
```

## Execute this file

```txt
$ codedown forth < assertions.md | grep . | gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
: add1 ( n -- n ) 1 + ;  ok
: add1-test 2 add1 assert( 3 = ) ;  ok
add1-test  ok
```
