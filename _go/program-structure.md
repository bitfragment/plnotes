---
title: 'Go: program structure'
---

## Contents
{:.no_toc}

* TOC
{:toc}


## Sources

Alan A. A. Donovan and Brian W. Kernighan, *[The Go Programming Language].*
Addison-Wesley, 2016, Chapter 2: Program Structure

[The Go Programming Language]: http://www.gopl.io/


## Setup

```go
package main

import (

)
```

## Conventions

* Use short names
* Use camelCase
* Handle an error and return — don't indent (e.g. enclose in an `else` block) the successful execution path


## File format and syntax

* The case of an identifier determines visibility: uppercase identifiers are exported, becoming visible outside the package
* Declaration order:
    - **Package** declaration
        + This is required, but need not correspond to the filename
        + Package names are always lowercase
        + `package main` defines a standalone executable
        + A standalone executable must have a `func main()`
    - **Import** declarations
    - **Package-level** declarations
* Single import declaration: `import "fmt"`
* Package-level declarations may be in any order
* Four package-level declarations:
    - `var`
        + `var <name> <type> = <expression>`
        + You can omit the type *or* the expression, not both
        + If an expression is omitted, the variable will be initialized to a standard **zero value**: there are no uninitialized variables
            * 0 for numbers
            * `""` (empty string) for string
            * `nil` otherwise
    - `const`
    - `type`
    - `func`
* Multiple declarations on one line
    - `var a, b, c int`
    - Initializing: `var a, b, c = 1, 2.3, "foo"`
    - Initializing with a function returning multiple values: `var a, b = f()`
* Short variable declaration *inside function only*
    - `a := 1`
    - Convention is to use these: save `var` declarations inside a function for when you need to specify a type
    - Multiple: `a, b := 1, 2`
        + Will reassign within the same lexical block
* Tuple assignment: `a, b = b, a`

Preferred style for multiple declarations is to enclose them in parentheses:

```go
import (
    "fmt"
    "os"
)
```

```go
const (
    a = 1
    b = 1
)
```


## Package structure

* `import ./foo` will import the contents of every file in directory `./foo`
* You then use whatever package names were declared in those files, e.g., `package mypkg` → `mypkg.myname`
* Package-level names are visible to all other files of the package, as if they composed a single file
* By convention, the final segment of the *import path string* (an argument to `import`) and the *package name* (an argument to `package`) are identical


## Pointers

* `*p` and `&p` have been retained, but there is no pointer arithmetic

It is safe to return the address of a local variable:

```go
func f() * int {
    a := 1
    return &a
}

func main() {
    fmt.Println(*f()); // => 1
}
```


## Types

* Declared at package level
* `type <name> <standard type>`
* Casting with `myType(value)`
* Use case: with Fahrenheit and Celsius values to prevent them from being compared or combined directly


## Scope

* *Scope* is a region of text at compile time; *lifetime* is an interval of referrability in run time
* The entire source of a program is considered a *universe block* having what in other languages we would call global scope
* At package level, the order of declaration has no effect on scope: a declaration may refer to itself, or to a declaration that follows it
* `init()` functions will be automatically called in the order in which they are declared, once the program starts. This is useful for e.g. precomputing a table of values
* Programs are lexically scoped within blocks
* "Inner" (inside a nested block) declarations of a given name will *shadow* or *hide* "outer" declarations of the same name
* Use `var` if you need to declare a universe-block (i.e. global) variable inside a function


## `main()`

```go
func main() {
    server4()
}
```


## Execute this file

```txt
$ codedown go < input-output.md | grep . > /tmp/tmp.go && go run /tmp/tmp.go &
$ open http://localhost:8000
```