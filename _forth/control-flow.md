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

[Starting Forth]: https://www.forth.com/starting-forth/

## Operator equivalents

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
1 >0        0= s" fail " exception and throw
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
