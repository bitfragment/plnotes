---
title: 'Forth implementations and tools'
---

## Contents
{:.no_toc}

* TOC
{:toc}


## Sources

* [Gforth Manual] — [5.24 Programming Tools]

[Gforth Manual]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/
[5.24 Programming Tools]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/Programming-Tools.html#Programming-Tools

## Gforth

### Install 

```txt
$ brew install gforth
```

### Use interactively
 
```txt
$ gforth
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
2 2 + . 4  ok
bye
```

### Run file from command line

```txt
$ gforth basics.forth
```

### Interpreter and system state

* `page` to clear display
* `cold` to restart Forth environment
* `bye` to exit Forth environment to OS

### Print debugging

The word `~~` will print the source line and stack contents at that point.

```forth
: f ( -- n ) clearstack 1 ~~ 2 ~~ ;
f
```

```forth
/Users/blennon/temp/temp.forth:1:<1> 1
/Users/blennon/temp/temp.forth:1:<2> 1 2
```

### Stepping through code

Gforth includes a [single-step debugger]. You must invoke the `gforth-itc`
engine:

[single-step debugger]: https://www.complang.tuwien.ac.at/forth/gforth/Docs-html/Singlestep-Debugger.html#Singlestep-Debugger

```txt
$ gforth-itc
Gforth 0.7.3, Copyright (C) 1995-2008 Free Software Foundation, Inc.
Gforth comes with ABSOLUTELY NO WARRANTY; for details type `license'
Type `bye' to exit
: add1 1 + ;  ok
2 dbg add1
: add1
Scanning code...

Nesting debugger ready!
[ 1 ] 00002
10997E270 10989C560 1              -> [ 2 ] 00002 00001
10997E280 10989C568 +              -> [ 1 ] 00003
10997E288 10989C428 ;              ->  ok
bye
```

`break:` in source code will invoke the debugger. Can only be used in
compile-time (colon) definitions.

```forth
: f ( n -- n ) 1 2 + 3 break: * 4 / ;
0 f .
bye
```

```txt
$ gforth-itc ~/temp/temp.forth

Scanning code...
<4447085322> <0>
Nesting debugger ready!
[ 3 ] 00000 00003 00003
1092242C8 1091405B8 *              -> [ 2 ] 00000 00009
1092242D0 109140560 4              -> [ 3 ] 00000 00009 00004
1092242E0 1091405C0 /              -> [ 2 ] 00000 00002
1092242E8 109140428 ;              -> 2
```

`break" string"` will include a label.

```forth
: f ( n -- n ) 1 2 + 3 break" multiplication" * 4 / ;
0 f .
bye
```

```txt
$ gforth-itc ~/temp/temp.forth

BREAK AT: multiplication

Scanning code...
<4335170314> <0>
Nesting debugger ready!
[ 3 ] 00000 00003 00003
102768308 1026855B8 *              -> [ 2 ] 00000 00009
102768310 102685560 4              -> [ 3 ] 00000 00009 00004
102768320 1026855C0 /              -> [ 2 ] 00000 00002
102768328 102685428 ;              -> 2
```