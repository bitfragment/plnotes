---
title: 'Forth exceptions'
---

## Contents
{:.no_toc}

* TOC
{:toc}


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
