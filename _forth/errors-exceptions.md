---
title: 'Forth errors and exceptions'
---

## Contents
{:.no_toc}

* TOC
{:toc}

## Sources

* [Gforth Manual] — [5.8.6 Exception Handling]

[Gforth Manual]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/
[5.8.6 Exception Handling]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/Exception-Handling.html

## Catching exceptions

Here the stack is empty, so `.` triggers stack underflow.

```forth
clearstack . 
:21: Address alignment exception
clearstack >>>.<<<
Backtrace:
$104EB8728 dup 
$104EB9E50 s>d 
```

Catch the exception.

```forth
clearstack '. catch  ok
```

## Throwing errors and exceptions

> A common idiom to THROW a specific error if a flag is true is this:
> `( flag ) 0<> errno and throw` (Gforth Manual 5.8.6 Exception Handling)

If a flag is false:

`( flag ) 0= errno and throw`

To provide a message:

`( flag ) 0= s" msg" exception  and throw`

```forth
5 2 / 2 = 0= s" not equal" exception and throw  ok
5 2 / 1 = 0= s" not equal" exception and throw 
:3: not equal
5 2 / 1 = 0= s" not equal" exception and >>>throw<<<
Backtrace:
```