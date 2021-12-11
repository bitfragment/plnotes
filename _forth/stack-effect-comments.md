---
title: 'Forth stack effect comments'
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


## In word definitions

Format: `( before -- after )`, with "before" meaning what is at the top
of the stack before the call, and "after" meaning what is at the top of
the stack after the call.

```forth
: add5 ( n -- n ) 5 + ;
1 add5 .
```
