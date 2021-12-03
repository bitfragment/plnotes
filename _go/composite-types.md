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


## Slices

Here `os.Args` is a slice (a dynamically sized array) of strings. We concatenate the slice elements into a single string.

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

* A slice is a variable-length sequence that provides access to an underlying array

* A slice is composed of a pointer, a length, and a capacity
    - The *length*, accessed by `len()`, is the number of elements in the slice
    - The *capacity*, accessed by `cap()`, is the number of elements between the start of the slice and the end of the underlying array
* The slice operator `s[i:j]` creates a new slice referring to elements `i` through `j` of `s` (which could be an array, a pointer to an array, or another slice)
    - `s[:3]` includes elements from index 0–3
    - `s[3:]` includes elements from index 3 to the end of `s`
    - `s[:]` includes the entirety of `s`

You can *extend a slice* beyond its own length (though *not* beyond the capacity
of the underlying array):

```golang
x := [...]int{1, 2, 3} // [1 2 3]
y := x[:1]             // [1]
z := y[:2]             // exceeds y's length! => [1 2]
```


## Maps

* A map is a reference, and a function receives a copy of the reference.
* No key errors to worry about. The map is initialized with a zero
  value for each key's data type.

Demonstrated here using a variation on Unix `uniq`.
Note, does not handle errors from `input.Scan()`, but should.

```go
func dup1() {
	counts := make(map[string]int)
	input := bufio.NewScanner(os.Stdin)
	for input.Scan() {
		counts[input.Text()]++
	}
	for line, n := range counts {
		if n > 1 {
			fmt.Printf("%d\t%s\n", n, line)
		}
	}
}
```


## Sources

Alan A. A. Donovan and Brian W. Kernighan, *[The Go Programming Language].*
Addison-Wesley, 2016, Chapter 4: Composite Types

[The Go Programming Language]: http://www.gopl.io/
