---
title: 'Go: composite types'
---

## Contents
{:.no_toc}

* TOC
{:toc}

## Arrays

* Arrays have a fixed length and are rarely used
* Syntax: `var a [3]int`
* Are subscriptable
* `len` returns the number of elements
* Initialization with array literal: `var a [3]int = [3]int{1, 2, 3}`
    - You can write an ellipsis instead of the length declaration:
      `var a [...]int = [3]int{1, 2, 3}`
* Arrays are comparable if their element types are comparable:
  `[...]int{1, 2, 3} == [...]int{1, 2, 3} // => true`
* Arrays are passed by value, not reference: a function receives a copy of an
  array argument, not a reference
    - If you don't want this behavior, you can pass a pointer to an array

## Sources

Alan A. A. Donovan and Brian W. Kernighan, *[The Go Programming Language].*
Addison-Wesley, 2016, Chapter 4: Composite Types

[The Go Programming Language]: http://www.gopl.io/
