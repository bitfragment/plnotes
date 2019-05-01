---
title: 'Go: getting oriented'
---

* TOC
{:toc}


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
* Single import declaration: `import "fmt"`
* Functions and other package-level declarations may be in any order

Preferred style for multiple import declarations: 

```go
import (
    "fmt"
    "os"
)
```

* No semicolons
* Comments are `//` and `/* ... */`
* `if` and `for` conditions are not parenthesized
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
    

## Strings

* `import "strings"`
* split: `strings.Split(mystring, "\n")`
* join: `strings.Join(arr, " ")`


## `for` loop

* Syntax `for <init>; <cond>; <post> {}`
* No parentheses
* All three loop statements are optional
* While loop is written as `for <cond> {}`
* Infinite loop: `for {}`
* For [FIXME: iterator], just use e.g. `for input.Scan() { /* do something with `input` */ }`

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


## I/O

* `fmt.Println()`
* `fmt.Printf()` with e.g. `%0.2f` to format a float to two decimal points of 
  precision
* `fmt.Fprintf()` ← TODO
* `os.Args` is an array of command-line arguments
* Use `bufio.Scanner()`, `io/ioutil.ReadFile()`, and `io/ioutil.WriteFile()` (they
  are implemented using `*os.File.Read()` and `*os.File.Write()`, which rarely need
  to be used)
* stdin
    - `input.Scan()`
        - removes newline
        - returns `true` if there is a line
        - returns `false` if there is no more input
    - get stdin:
        1. `input := bufio.NewScanner(os.Stdin)`
        2. `for input.Scan() { /* do something with input.Text() */ }`
* read file data in streaming mode
    - `os.Open()` returns (a) a file handle and (b) an error value
    - read file data:
        1. `file, err := os.Open("./data.txt")`
        2. Check `err`. If it's `nil`, all is well.
        3. `data := bufio.NewScanner(file)`
        4. `for data.Scan() { /* do something with data.Text() */ }`
        5. Close the file with `file.Close()`.
* read file data in one operation, using `io/ioutil.ReadFile()`
    - `io/ioutil.ReadFile()` returns (a) a *byte slice* and (b) error value
    - read file data:
        1. `data, err := ioutil.ReadFile("./data.txt")`
        2. Check `err`
        3. Do something with `data`: e.g. process it as newline-terminated strings:
            - `strdata := strings.Split(string(data), "\n")`
            - `for _, line := range strdata { /* do something with `line` */ } `


Read file data in streaming mode (checks file error only, not scanner error):

```golang
file, err := os.Open("./data.txt")
if err != nil {
    fmt.Println("Error")
    return
}
data := bufio.NewScanner(file)
for data.Scan() {
    fmt.Println(data.Text())
}
```

Read file data in one operation (omits error checking):

```golang
data, err := ioutil.ReadFile("./data.txt")
strdata := strings.Split(string(data), "\n")
for _, line := range strdata {
    fmt.Println(line)
}
```


## Data structures: map

* Create with `make`, e.g. `make(map[string]int)` initializes a map with
  string keys and integer values, with keys set to zero values of `""` and
  values set to zero values of 0.
* Order of map iteration is not specified and in practice is random.

Creating, adding elements to, and iterating over a map:

```go
counts := make(map[string]int)
counts["foo"]++
counts["bar"]++
counts["bar"]++
counts["baz"]++
counts["baz"]++
counts["baz"]++
for elt, idx := range counts {
    fmt.Println(elt, idx)
}
```

Output (order not guaranteed):

```
foo 1
bar 2
baz 3
```

Taking standard input:

```go
counts := make(map[string]int)
input := bufio.NewScanner(os.Stdin)
for input.Scan() {
    counts[input.Text()]++
}
for elt, idx := range counts {
    fmt.Println(elt, idx)
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
