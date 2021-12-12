---
title: 'Forth stack manipulation'
---

## Contents
{:.no_toc}

* TOC
{:toc}

## Sources

* Leo Brodie, *[Starting FORTH]* Chapter 2: How to Get Results

[Starting FORTH]: https://www.forth.com/starting-forth/

## `swap`

Swap the top two stack items. `( n1 n2 -- n2 n1 )`

```forth
1 2 swap 1 = 0= s" test failed " exception and throw
```

## `dup`

Duplicate the top stack item. `( n -- n n )`

```forth
clearstack 1 dup + 2 = 0= s" test failed " exception and throw
```

## `over`

Copy the second item on the stack to its top. `( n1 n2 -- n1 n2 n1 )`

```forth
clearstack 1 2 over 1 = 0= s" test failed " exception and throw
```

## `rot`

Rotate the third item on the stack to its top. `( n1 n2 n3 -- n2 n3 n1 )`

```forth
clearstack 1 2 3 rot 1 = 0= s" test failed " exception and throw
```

## `drop`

Drop the top item. `( n -- )`

```forth
clearstack 1 2 drop 1 = 0= s" test failed " exception and throw
```

## Double-cell stack operators

### `2swap`

Reverse the top two pairs of numbers. `( d1 d2 -- d2 d1 )`

```forth
clearstack 1 2 3 4 2swap + 3 = . + 7 = 0= s" test failed " exception and throw
```

### `2dup`

Duplicate the top pair of numbers. `( d -- d d )`

```forth
clearstack 1 2 2dup + 3 = . + 3 = 0= s" test failed " exception and throw
```

### `2over`

Duplicate the second pair of numbers. `( d1 d2 -- d1 d2 d1 )`

```forth
clearstack 1 2 3 4 2over + 3 = 0= s" test failed " exception and throw
```

### `2drop`

Discards the top pair of numbers. `( d1 d2 -- d1 )`

```forth
clearstack 1 2 3 4 2drop + 3 = 0= s" test failed " exception and throw
```

## Execute this file

```txt
$ codedown forth < stack-manipulation.md | grep . | gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
1 2 swap 1 = 0= s" test failed " exception and throw  ok
clearstack 1 dup + 2 = 0= s" test failed " exception and throw  ok
clearstack 1 2 over 1 = 0= s" test failed " exception and throw  ok
clearstack 1 2 3 rot 1 = 0= s" test failed " exception and throw  ok
clearstack 1 2 drop 1 = 0= s" test failed " exception and throw  ok
clearstack 1 2 3 4 2swap + 3 = . + 7 = 0= s" test failed " exception and throw -1  ok
clearstack 1 2 2dup + 3 = . + 3 = 0= s" test failed " exception and throw -1  ok
clearstack 1 2 3 4 2over + 3 = 0= s" test failed " exception and throw  ok
clearstack 1 2 3 4 2drop + 3 = 0= s" test failed " exception and throw  ok
```