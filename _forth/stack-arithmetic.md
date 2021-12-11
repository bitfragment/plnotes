---
title: 'Forth stack arithmetic'
---

## Contents
{:.no_toc}

* TOC
{:toc}


## Sources

* Leo Brodie, *[Starting FORTH],* 
  * Chapter 1: Fundamental Forth
  * Chapter 2: How to Get Results

[Starting FORTH]: https://www.forth.com/starting-forth/

## Notation

Use postfix notation.

## Integer arithmetic operators

Limited to single-precision integers.

```forth
2 2 + 4 = .
2 2 - 0 = .
2 2 * 4 = .
2 2 / 1 = .
```

You can't use decimal values with integer arithmetic operators.

```forth
2.0 2.0 + 4.0 = .
```

You can only get integer results of division.

```forth
5 2 / .
```

### Examples: addition

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

The following are equivalent.

In the first, 1 and 2 are pushed onto the stack, then popped, summed,
and the result is pushed onto the stack. Then, that result and 3 are
popped, summed, and the result is pushed onto the stack. Then, that
result and 4 are popped, summed, and so on.

Q: what is the correct way to explain stack operations in the second case?

```forth
1 2 + 3 + 4 + 5 + .
1 2 3 4 5 + + + + .
```

### Examples: multiple operators

This is the equivalent of the equation *(a + b) * c* where *a* = 1,
*b* = 2, *c* = 3.

```forth
3 1 2 + * .
```

Here, 3 and 9 are pushed onto the stack, then popped, summed, and the
result is pushed onto the stack. Then, 4 and 6 are pushed onto the 
stack, then popped, summed, and the result is pushed onto the stack.
Finally, the first two values on the stack are popped, multiplied, and
the result is pushed onto the stack.

```forth
3 9 + 4 6 + * .
```

## Execute this file

```txt
$ codedown forth < stack-arithmetic.md | grep . | gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
2 2 + 4 = . -1  ok
2 2 - 0 = . -1  ok
2 2 * 4 = . -1  ok
2 2 / 1 = . -1  ok
2.0 2.0 + 4.0 = . 0  ok
5 2 / . 2  ok
2 2 + . 4  ok
: add4 4 + ;  ok
1 add4 . 5  ok
1 2 + 3 + 4 + 5 + . 15  ok
1 2 3 4 5 + + + + . 15  ok
3 1 2 + * . 9  ok
3 9 + 4 6 + * . 120  ok
```