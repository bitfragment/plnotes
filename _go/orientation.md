---
title: 'Go: getting oriented'
---

## Contents
{:.no_toc}

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

* I installed MacOS Homebrew **golang**
* `go build myprogram.go` to compile and link
* `go run myprogram.go` to compile, link and run
* `go fmt` to format code (alphabetizes import declarations)
* `go vet` to check code
* `go test`
* `go test -bench` for benchmarking
* Remote packages:
    - `go get` to install (`-v` for verbose, `-u` to update)
        + Mine (MacOS) installs in `~/go` by default
            * This represents default `$GOPATH` ?
        + Therefore add `$HOME/go/bin` to your shell `$PATH`
    - `go list ...` to list all installed packages
* Tools not built in:
    - `golang.org/x/tools/cmd/goimports` updates all import statements,
      adding needed or removing unused
* Playground: https://play.golang.org/
* Currently maintained REPLs I've tried (as of 2019-05-08)
    - gomacro <https://github.com/cosmos72/gomacro>
        + My preference. Fast; has integrated debugger
    - gore <https://github.com/motemen/gore> was too slow for me
    - go-pry <https://github.com/d4l3k/go-pry> - I did not try it


## File format and syntax

* No semicolons
* Comments are `//` and `/* ... */`
* `if` and `for` conditions are not parenthesized
* `+` is overloaded for string concatenation
* `+=` is valid and has all the usual variants
* Go has postfix only `++` and `--`
    - these are statements, not expressions as in C, so `j = i++` is invalid


## Assertions

Not provided. Use `if <failing_cond> { panic("failed") }` instead.


## Variable declarations

Use `:=` operator to initialize, `=` to assign.

The following four ways to declare and initialize variables are 
equivalent.

> In practice, you should generally use one of the first two forms,
> with explicit initialization to say that the initial value is
> important and implicit initialization to say that the initial value
> doesn't matter.
—Donovan and Kernighan, *The Go Programming Language* 7

First: short variable declaration. Declare and initialize without `var` 
keyword or type, using `:=`. This form may only be used within a 
function.

```go
a := "foo"
if a != "foo" { panic("failed") }
```

Second: declare with type. Here, is default-initialized with empty string.

```go
var b string
if b != "" { panic("failed") }
```

Assign another value using `=` operator.

```go
b = "bar"
if b != "bar" { panic("failed") }
```

Third: declare and initialize with a value, using `=`.

```go
var c string = "baz"
if c != "baz" { panic("failed") }
```

Fourth: declare and initialize without type, using `=`. "Rarely used 
except when declaring multiple variables" (Donovan and Kernighan, 
*The Go Programming Language* 7).

```go
var d = "qux"
if d != "qux" { panic("failed") }
```


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
func echo() {
	s, sep := "", ""
	for _, arg := range os.Args[1:] {
		s += sep + arg
		sep = " "
	}
	fmt.Println(s)
}
```

Rather than use the blank identifier for the value variable, you can simply omit it:
`for idx := range arr` is equivalent to `for idx, _ := range arr`.

Rather than string concatenation, use `strings.Join`:

```go
func echo() {
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


## Network I/O

* `net/http.Get(url`) returns `resp, err`; `resp` is a struct, `resp.Body` is a readable stream
* `io/ioutil.ReadAll(resp.Body)` returns `body, err`



## System calls

* `os.Exit()`
* `os.Stderr`



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
Addison-Wesley, 2016, Chapter 1: Tutorial

[The Go Programming Language]: http://www.gopl.io/
