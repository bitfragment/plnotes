---
title: "JavaScript syntax: core syntax"
---

I've highlighted here only those elements of JavaScript syntax that someone familiar with other languages (especially C-like languages) might want to know.

JavaScript line comments are demarcated by the `//` symbol. Block comments are formed using `/*` and `*/`.

Statements are semicolon-terminated. It's wise to forget about so-called optional semicolons and semicolon insertion by the interpreter.

Strings can be either single or double-quoted.

Variables are declared with `var`, or more recently, with `let` and `const`. `var` has **function scope, or else global scope**; `let` and `const` have **block** scope only.

Booleans are `true` and `false`.

`undefined` is the value assigned to a name to which no other value has yet been assigned. `null` is a value standing for no value.
