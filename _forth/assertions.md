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
define a test word `add1-test` that asserts a computable *flag* about
the value pushed onto the stack by `add1`. If the assertion fails,
execution stops; if the assertion succeeds, the value -1 (true) will
be left on the stack by that computation of the flag.

```forth
: add1 ( n -- n ) 1 + ;
: add1-test ( -- f ) 2 add1 assert( 3 = ) ;
add1-test
```

Outside of a colon definition, you can throw an exception if a computed
flag is not true with `( flag ) 0= s" msg" exception  and throw`.

```forth
5 2 / 2 = 0= s" not equal" exception and throw
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
5 2 / 2 = 0= s" not equal" exception and throw  ok
```
