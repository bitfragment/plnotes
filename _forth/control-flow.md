---
title: 'Forth control flow'
---

## Contents
{:.no_toc}

* TOC
{:toc}

## Sources

* Leo Brodie, *[Starting Forth]: An Introduction to the FORTH Language
  and Operating System for Beginners and Professionals,* online edition,
  Chapter 4: Decisions, Decisions...
* Edward K. Conklin and Elizabeth D. Rather, *[Forth Programmer's
  Handbook]* 3rd edition, Chapter 4 Structured Programming

[Starting Forth]: https://www.forth.com/starting-forth/
[Forth Programmer's Handbook]: https://www.forth.com/product/forth-programmers-handbook/

## Operator equivalents

These leave either a nonzero (true) or a zero (false) on the stack.

`=` equal, `<>` not equal, `>` and `<`.

```forth
1 1 =       0= s" fail " exception and throw
1 2 <>      0= s" fail " exception and throw
1 2 <       0= s" fail " exception and throw
2 1 >       0= s" fail " exception and throw
```

`0=` equals zero, `0<` negative, `0>` positive.

```forth
0 0=        0= s" fail " exception and throw
-1 0<       0= s" fail " exception and throw
1 0>        0= s" fail " exception and throw
```

## Conditional phrasing

Compile-time only, so can only be used in word definitions.

`if` evaluates any non-zero value as true, and proceeds.

Here, push 1 onto the stack and evaluate it using `if`. Because 1 is
truthy, push 2 on to the stack.

```forth
: f ( -- 2 ) 1 if 2 then ;
: f-test ( -- ) f assert( 2 = ) ;
f-test
```

Note that `if` modifies the stack. I did not understand this at first.

> When `IF` is executed, the item on top of the stack is *removed* and
> examined. (Conklin and Rather, *Forth Programmer's Handbook* 100)

Demo via debugger:

```forth
: test if ." NONZERO " else ." ZERO" then ; ok
-1 dbg test 
: test  
Scanning code...

Nesting debugger ready!
[ 1 ] 18446744073709551615 
101E20710 1017E7458 IF             -> [ 0 ] 
101E20720 1017E7450 .\" NONZERO "  -> NONZERO [ 0 ] 
101E20768 1017E7450 ELSE           -> [ 0 ] 
101E207C0 1017E7428 THEN ;         ->  ok

0 dbg test  
: test  
Scanning code...

Nesting debugger ready!
[ 1 ] 00000 
101E20710 1017E7458 IF             -> [ 0 ] 
101E20778 1017E7450 .\" ZERO"      -> ZERO[ 0 ] 
101E207C0 1017E7428 THEN ;         ->  ok

1 dbg test 
: test  
Scanning code...

Nesting debugger ready!
[ 1 ] 00001 
101E20710 1017E7458 IF             -> [ 0 ] 
101E20720 1017E7450 .\" NONZERO "  -> NONZERO [ 0 ] 
101E20768 1017E7450 ELSE           -> [ 0 ] 
101E207C0 1017E7428 THEN ;         ->  ok

```

## invert

Reverses the flag on the stack.

Here, push 0 onto the stack and evaluate it using `if`. 0 is falsy, but
we invert the flag on the stack.

```forth
: f ( -- 2 ) 0 invert if 2 then ;
: f-test ( -- ) f assert( 2 = ) ;
f-test
```

## else

It's logically straightforward but I find it difficult to use.

The way to read it is to focus on the words between `if` and `then`.

Here, `if` evaluates the value on the stack. If that value is truthy,
`true` is pushed onto the stack. If that value is falsy, at `else`,
`false` is pushed onto the stack.

```forth
: ?truthy ( any -- boolean ) if true else false then  ;
```

Here, evaluate the value on the stack. If it's < 11, increment it by 1.
Otherwise, decrement it by 1.

```forth
: f  ( n -- n ) dup 11 < if 1 + else 1 - then ;
: f-test ( -- ) 5 f assert( 6 = ) clearstack 11 f assert( 10 = ) ;
f-test
```

Debug:

```txt
$ gforth-itc
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
: f  ( n -- n ) dup 11 < if 1 + else 1 - then ;  ok
5 dbg f 
: f  
Scanning code...

Nesting debugger ready!
[ 1 ] 00005 
104B9D270 104AB9838 dup            -> [ 2 ] 00005 00005 
104B9D278 104AB9560 11             -> [ 3 ] 00005 00005 00011 
104B9D288 104AB96C0 <              -> [ 2 ] 00005 18446744073709551615 
104B9D290 104AB9458 IF             -> [ 1 ] 00005 
104B9D2A0 104AB9560 1              -> [ 2 ] 00005 00001 
104B9D2B0 104AB9568 +              -> [ 1 ] 00006 
104B9D2B8 104AB9450 ELSE           -> [ 1 ] 00006 
104B9D2E0 104AB9428 THEN ;         ->  ok
clearstack 11 dbg f 
: f  
Scanning code...

Nesting debugger ready!
[ 1 ] 00011 
104B9D270 104AB9838 dup            -> [ 2 ] 00011 00011 
104B9D278 104AB9560 11             -> [ 3 ] 00011 00011 00011 
104B9D288 104AB96C0 <              -> [ 2 ] 00011 00000 
104B9D290 104AB9458 IF             -> [ 1 ] 00011 
104B9D2C8 104AB9560 1              -> [ 2 ] 00011 00001 
104B9D2D8 104AB9580 -              -> [ 1 ] 00010 
104B9D2E0 104AB9428 THEN ;         ->  ok
```

## and, or

Work as you would expect.

```forth
: test-bools
  clearstack true true and assert( true = )
  clearstack true false and assert( false = )
  clearstack true true or assert( true = )
  clearstack true false or assert( true = )
  clearstack false false or assert( false = ) ;
test-bools
```

## ?dup

Duplicates the top stack value only if it is non-zero.

```forth
: test-?dup
  clearstack 1 ?dup + assert( 2 = )
  clearstack 0 ?dup assert( 0 = ) ;
test-?dup
```

## abort

Aborts if the flag on the stack is true. Compile-time only.

```txt
$ gforth-itc
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
: test-abort 1 abort" aborted " ;  ok
test-abort  
:2: aborted 
>>>test-abort<<< 
Backtrace:
$10DC789A8 throw 
$10DCCB2D8 c(abort") 
.s <0>  ok
```

## Execute this file

```txt
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
1 1 =       0= s" fail " exception and throw  ok
1 2 <>      0= s" fail " exception and throw  ok
1 2 <       0= s" fail " exception and throw  ok
2 1 >       0= s" fail " exception and throw  ok
  ok
0 0=        0= s" fail " exception and throw  ok
-1 0<       0= s" fail " exception and throw  ok
1 0>        0= s" fail " exception and throw  ok
  ok
: f ( -- 2 ) 1 if 2 then ;  ok
: f-test ( -- ) f assert( 2 = ) ;  ok
f-test  ok
  ok
: f ( -- 2 ) 0 invert if 2 then ;  ok
: f-test ( -- ) f assert( 2 = ) ;  ok
f-test  ok
  ok
: ?truthy ( any -- boolean ) if true else false then  ;  ok
  ok
: f  ( n -- n ) dup 11 < if 1 + else 1 - then ;  ok
: f-test ( -- ) 5 f assert( 6 = ) clearstack 11 f assert( 10 = ) ;  ok
f-test  ok
  ok
: test-bools  compiled
  clearstack true true and assert( true = )  compiled
  clearstack true false and assert( false = )  compiled
  clearstack true true or assert( true = )  compiled
  clearstack true false or assert( true = )  compiled
  clearstack false false or assert( false = ) ;  ok
test-bools  ok
  ok
: test-?dup  compiled
  clearstack 1 ?dup + assert( 2 = )  compiled
  clearstack 0 ?dup assert( 0 = ) ;  ok
test-?dup  ok
```