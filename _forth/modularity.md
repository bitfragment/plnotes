---
title: 'Forth modularity'
---

## Contents
{:.no_toc}

* TOC
{:toc}

## Sources

Leo Brodie, *[Thinking Forth]: A Language and Philosophy for Solving 
Problems,* 2004 Edition, Chapter 1: The Philosophy of Forth

[Thinking Forth]: http://thinking-forth.sourceforge.net/

## Significance

Modularity is "Forth's most significant breakthrough" in that

> the smallest atom of a Forth program is not a module or a subroutine
> or a procedure, but a "word." Furthermore, there are no subroutines,
> main programs, utilities, or executives, each of which must be invoked
> differently. *Everything* in Forth is a word. (Brodie, *Thinking
> Forth* 19)

## Information hiding

Suppose that we have defined a variable `apples` as follows, and used it
extensively in a program.

```forth
variable apples
```

But over time, the application of the program changes, and we find that
instead of a single category `apples`, we ought to have started with
two categories: red apples and green apples. Instead of rewriting the
program, Brodie suggests that we redefine `apples` to "supply two
different [memory] addresses depending on which kind of apple we're 
currently talking about" (25).

```forth
variable color  ( pointer to current tally )
variable reds   ( tally of red apples )
variable greens ( tally of green apples )
: red    ( set apple type to red ) reds color ! ;
: green  ( set apple type to green) greens color ! ;
: apples ( -- adr of current apple tally ) color @ ;
```

`apples` is no longer a variable; now it is a colon definition whose
function is to get the contents of the variable `color`, which stores
the memory address of either the variable `reds` or the variable `greens`.
Now we can keep tallies of both red and green apples.

```forth
5 red apples !
10 green apples !
```

Let's define some test words to verify the current tallies:

```forth
: test-red ( n -- ) assert( reds @ = ) ;
: test-green ( n -- ) assert( greens @ = ) ;
5 test-red
10 test-green
```

This does not change the original usage of `apples` in existing code. We
can continue using the word `apples` as before. Now, via the contents of
the variable `color`, it gets the tally of whatever color apples are
currently set to: that is, whichever of `red` or `green` was most
recently called. We can continue using it with that reference.

```forth
: test-apples ( n -- ) assert( apples @ = ) ; 
10 test-apples
1 apples +!
11 test-apples
11 test-green
```

Brodie, *Thinking Forth* 26:

> Look again at what we did. We changed the definition of `APPLES` from
> that of a variable to a colon definition, without affecting its usage.
> Forth allows us to hide the details of how `APPLES` is defined from
> the code that uses it. What appears to be "thing" (a variable) to the
> original code is actually defined as an "action" (a colon definition)
> within the component. [...] Only Forth, which eliminates the `CALL`s
> from procedures, which allows addresses and data to be implicitly
> passed via the stack, and which provides direct access to memory
> locations with `@` and `!`, can offer this level of
> information-hiding. Forth pays little attention to whether something
> is a data structure or an algorithm.

## Execute this file

```txt
$ codedown forth < modularity.md | grep . | gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
variable apples  ok
variable color  ( pointer to current tally )  ok
variable reds   ( tally of red apples )  ok
variable greens ( tally of green apples )  ok
: red    ( set apple type to red ) reds color ! ;  ok
: green  ( set apple type to green) greens color ! ;  ok
: apples ( -- adr of current apple tally ) color @ ; redefined apples   ok
5 red apples !  ok
10 green apples !  ok
: test-red ( n -- ) assert( reds @ = ) ;  ok
: test-green ( n -- ) assert( greens @ = ) ;  ok
5 test-red  ok
10 test-green  ok
: test-apples ( n -- ) assert( apples @ = ) ;   ok
10 test-apples  ok
1 apples +!  ok
11 test-apples  ok
11 test-green  ok
```