---
title: 'Go: basic data types'
---

## Contents
{:.no_toc}

* TOC
{:toc}

Basic data types are boolean, numeric, string.

## Comparability

* All basic types are comparable using `==` and `!=`
* All basic types are ordered by comparison operators `<`, `>`, etc.

## Printing

* `fmt.Printf()` uses C-style specifiers
* Example: `fmt.Printf("%08b", 1)` → print binary representation of int value 1,
  padding with zeroes to width of 8 ("00000001")

## Numeric types

* Implicitly signed and unsigned: `int`, `uint`
    - These are implementation-dependent platform-specific default or natural sizes (either 32 or 64 bits)
    - `int` is a different type than `int32`, even if they have the same natural size on a given platform. You will still need to convert them.
* Explicitly signed: `int8`, `int16`, `int32`, `int64`
    - Convention is to use these by default; use unsigned for binary operations
    - Use two's complement representation
* Explicitly unsigned: `uint8`, `uint16`, `uint32`, `uint64`
* `float32` and `float64`
    - Convention is to use `float64`
* `byte` is for raw data, and is equivalent to `uint8`
* `rune` is for a Unicode code point and is equivalent to `int32`
    - Rune literals are single-quoted Unicode characters: `const x rune = '🤖'`
    - Use `fmt.Printf()` with `%c` to get character representation;
      `fmt.Print()` will print the integer code point value
* `uintptr` is for pointer values (implementation-dependent)
* `math.isNaN()`
* `math.NaN()` returns `NaN`, but don't compare it: comparisons always yield `false`

### Conversion

* Conversion using e.g. `int()`: 
  `const a int8 = 1; const b int16 = 2; const c int = int(a) + int(b)`

### Numeric operations

* `/` with two integer operands will truncate
* As binary infix operator `^` is bitwise XOR
* As unary prefix operator, `^` is bitwise negation or complement
* `&^` is AND NOT (bit clear)
* Left-shift zero-pads
* Right-shift of an unsigned value zero-pads
* Right-shift of a signed value pads with the sign bit

## Sources

Alan A. A. Donovan and Brian W. Kernighan, *[The Go Programming Language].*
Addison-Wesley, 2016, Chapter 3: Basic Data Types

[The Go Programming Language]: http://www.gopl.io/
