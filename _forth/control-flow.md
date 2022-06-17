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

## Boolean values

Logical evaluation leaves a boolean *flag* on the stack. Nonzero values,
including both 1 and -1, are interpreted as *true*, while a zero value
is interpreted as *false*.

The words `true` and `false` can also be used.

```forth
: test-bools-words
  clearstack true true and assert( true = )
  clearstack true false and assert( false = )
  clearstack true true or assert( true = )
  clearstack true false or assert( true = )
  clearstack false false or assert( false = )
;
test-bools-words
```

However, when using these words, only the value -1 is interpreted as
`true`:

```forth
: test-bools-vals
  -1 -1 and assert( true = )
  -1  0 and assert( false = )
  -1 -1 or assert( true = )
  -1  0 or assert( true = )
   0  0 or assert( false = )
;
test-bools-vals
```

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

## Conditional phrasing: `if`...`else`...`then`

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

```txt
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

Because `if` evaluates any non-zero value as true, you can omit a
comparison operator when, for example, you want to check if a number
is zero:

```forth
: iszero ( n -- ) dup if ." NONZERO " else ." ZERO " drop then ;
```

Or if you want to check if two numbers are equal, here using
subtraction:

```forth
: eq ( n n -- ) - if ." NOT EQUAL " else ." EQUAL " then ;
```

### Flag arithmetic

You can use `swap` to combine flags from successive tests. Here, we
want a flag indicating whether a number is either negative or greater
than five.

1. `dup` the number so we can test it twice.
2. Test the number to see if it is negative. This will leave a logical
   flag on the stack.
3. `swap` the flag with the original number.
4. Test the number to see if it is greater than five.
5. We now have two flags on the stack. If we add them together, their
   sum is a flag indicating the result of a logical `or` for these two
   conditions.

```forth
: f ( n -- b ) dup 0< swap 5 > + ;

: test-f ( -- )
    -2 f assert( 0< )
    -1 f assert( 0< )
     0 f assert( 0= )
     1 f assert( 0= )
     2 f assert( 0= )
     5 f assert( 0= )
     6 f assert( 0< )
;
test-f
```

## Logical `and`, `or`

For cases where adding flags would not produce an accurate logical
result, there are the Forth words `and` and `or`. 

Here, we want to know if a number is greater than 5 *and* a multiple of 5:

```forth
: f ( n -- b ) dup 5 > swap 5 mod 0= and ;
: test-f ( -- )
    -1 f assert( 0= )
     0 f assert( 0= )
     1 f assert( 0= )
     5 f assert( 0= )
     6 f assert( 0= )
     9 f assert( 0= )
    10 f assert( 0< )
    11 f assert( 0= )
    15 f assert( 0< )
;
test-f
```

Here, we want to know if a number is less than -5 *or* greater than 5:

```forth
: f ( n -- b ) dup -5 < swap 5 > or ;
: test-f ( -- )
    -10 f assert( 0< )
     -5 f assert( 0= )
     -1 f assert( 0= )
      0 f assert( 0= )
      1 f assert( 0= )
      5 f assert( 0= )
      6 f assert( 0< )
     10 f assert( 0< )
;
test-f
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

## Words with included conditionals

`?dup` duplicates the top stack value only if it is non-zero:

```forth
: test-?dup
  1 ?dup + assert( 2 = )
  0 ?dup assert( 0 = ) ;
test-?dup
```

`abort` aborts if the flag on the stack is true. Compile-time only.
Useful for aborting execution in case of an error that you can test for
(e.g. division by zero).

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
[Running] codedown forth < control-flow.md | grep . | gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
: test-bools-words  compiled
  clearstack true true and assert( true = )  compiled
  clearstack true false and assert( false = )  compiled
  clearstack true true or assert( true = )  compiled
  clearstack true false or assert( true = )  compiled
  clearstack false false or assert( false = )  compiled
;  ok
test-bools-words  ok
: test-bools-vals  compiled
  -1 -1 and assert( true = )  compiled
  -1  0 and assert( false = )  compiled
  -1 -1 or assert( true = )  compiled
  -1  0 or assert( true = )  compiled
   0  0 or assert( false = )  compiled
;  ok
test-bools-vals  ok
1 1 =       0= s" fail " exception and throw  ok
1 2 <>      0= s" fail " exception and throw  ok
1 2 <       0= s" fail " exception and throw  ok
2 1 >       0= s" fail " exception and throw  ok
0 0=        0= s" fail " exception and throw  ok
-1 0<       0= s" fail " exception and throw  ok
1 0>        0= s" fail " exception and throw  ok
: f ( -- 2 ) 1 if 2 then ;  ok
: f-test ( -- ) f assert( 2 = ) ;  ok
f-test  ok
: iszero ( n -- ) dup if ." NONZERO " else ." ZERO " drop then ;  ok
: eq ( n n -- ) - if ." NOT EQUAL " else ." EQUAL " then ;  ok
redefined f  : f ( n -- b ) dup 0< swap 5 > + ;  ok
: test-f ( -- )  compiled
    -2 f assert( 0< )  compiled
    -1 f assert( 0< )  compiled
     0 f assert( 0= )  compiled
     1 f assert( 0= )  compiled
     2 f assert( 0= )  compiled
     5 f assert( 0= )  compiled
     6 f assert( 0< )  compiled
;  ok
test-f  ok
redefined f  : f ( n -- b ) dup 5 > swap 5 mod 0= and ;  ok
: test-f ( -- )  compiled
    -1 f assert( 0= )  compiled
     0 f assert( 0= )  compiled
     1 f assert( 0= )  compiled
     5 f assert( 0= )  compiled
     6 f assert( 0= )  compiled
     9 f assert( 0= )  compiled
    10 f assert( 0< )  compiled
    11 f assert( 0= )  compiled
    15 f assert( 0< )  compiled
;  ok
redefined test-f  test-f  ok
redefined f  : f ( n -- b ) dup -5 < swap 5 > or ;  ok
: test-f ( -- )  compiled
    -10 f assert( 0< )  compiled
     -5 f assert( 0= )  compiled
     -1 f assert( 0= )  compiled
      0 f assert( 0= )  compiled
      1 f assert( 0= )  compiled
      5 f assert( 0= )  compiled
      6 f assert( 0< )  compiled
     10 f assert( 0< )  compiled
;  ok
redefined test-f  test-f  ok
redefined f  : f ( -- 2 ) 0 invert if 2 then ;  ok
redefined f-test  : f-test ( -- ) f assert( 2 = ) ;  ok
f-test  ok
: test-?dup  compiled
  1 ?dup + assert( 2 = )  compiled
  0 ?dup assert( 0 = ) ;  ok
test-?dup  ok

[Done] exited with code=0 in 0.096 seconds
```