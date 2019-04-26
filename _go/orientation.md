---
title: 'Go: getting oriented'
---

## Hello world

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello 🌏")
}
```


## Tools

* `go build myprogram.go` to compile and link
* `go run myprogram.go` to compile, link and run
* `go fmt` to format code (alphabetizes import declarations)
* `go test`
* `go test -bench` for benchmarking


## File format and syntax

* `package` declaration is required (but need not correspond to the file name)
* `package main` defines a standalone executable
    - it must have a `func main()`
* single import declaration: `import "fmt"`

Preferred style for multiple import declarations: 

```go
import (
    "fmt"
    "os"
)
```

* No semicolons
* Comments are `//` and `/* ... */`
* `+` is overloaded for string concatenation
* `+=` is valid and has all the usual variants
* Go has postfix only `++` and `--`
    - these are statements, not expressions as in C, so `j = i++` is invalid
* Uninitialized variables are initialized to a *zero value*: 0 for numeric
  types, empty string for string
* Variable declaration with default zero-value initialization: `var s string`
* Multiple: `var a, b string`
* Variable declaration and initialization
    - Algol-style compact form (valid only inside functions): `x := 1`
    - `var s = "foo"` (valid at package level)
    

## I/O

* `import "fmt"`
* `fmt.Println()`
* `fmt.Printf()` with e.g. `%0.2f` to format a float to two decimal points of 
  precision
* `os.Args` is a subscriptable array of command-line arguments


## Strings

* `import "strings"` ... `strings.Join(arr, " ")`


## `for` loop

* Syntax `for <init>; <cond>; <post> {}`
* No parentheses
* All three loop statements are optional
* While loop is written as `for <cond> {}`
* Infinite loop: `for {}`

Range for loop (enumeration) uses `for idx, elt := range arr`. Go prohibits
unused local variables, so if you're not going to use the index variable, the
convention is to use the *blank identifier* `_`:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    var s, sep string
    for _, arg := range os.Args[1:] {
        s += sep + arg
        sep = " "
    }
    fmt.Println(s)
}
```

Rather than string concatenation, use `strings.Join`:

```go
package main

import (
    "fmt"
    "os"
    "strings"
)

func main() {
    fmt.Println(strings.Join(os.Args[1:], " "))
}
```


## Timing

* `start := time.Now()` ... `time.Since(start).Seconds()`



## Testing and benchmarking

* Test functions and benchmarking functions go in `<libname>_test.go`
* `import "testing"`
* Use `go test` and `go test -bench`

Writing a benchmark function for some function `f()`:

```go
func BenchmarkF1(b *testing.B) {
    for i := 0; i < b.N; i++ {
        f1()
    }
}
```

Run with `go test -bench=args1`.


## Sources

Alan A. A. Donovan and Brian W. Kernighan, *[The Go Programming Language].*
Addison-Wesley, 2016.

[The Go Programming Language]: http://www.gopl.io/
