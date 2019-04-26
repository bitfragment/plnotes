---
title: 'JavaScript types: numeric types'
---

## Representation

All numbers are represented as 64-bit IEEEE 754 floating-point values; there is no integer type. Integers between -2^53 and 2^53 can be represented exactly.

Array indexing and some bitwise operations may be performed with 32-bit integers.

## Literals

Hexadecimal literals are written with the `0x` or `0X` prefix. Some implementations may support octal literals written with the `0` prefix, but this is not supported by the ECMAScript standard, and ECMAScript 5 strict mode forbids octal literals. As usual, you should never write an integer literal with a leading zero unless you want an octal literal, and in the case of JavaScript, since they are unsupported or forbidden, you shouldn't use them at all.

Floating-point literals can be written in real number syntax (using a decimal point) or exponential notation. **JavaScript has no decimal numeric type like Java's `BigDecimal` class or Python's `decimal` module.**

## Arithmetic

Arithmetic operations are provided by `= - * / %` and by the methods of the `Math` object.

No errors are raised in cases of overflow, underflow, or division by zero. Overflow returns the global variable `Infinity` or `-Infinity`; underflow returns `0` or `-0`; division by zero returns `Infinity` or `-Infinity`, except in case of zero divided by zero, which returns the global variable `NaN.`

Division of `Infinity` by `Infinity`, taking the square root of a negative number, or arithmetic with non-coerceable non-numeric operands will also return `NaN`. 

## More on `NaN` and `-0`

`NaN` does not compare equal to any other value, **including itself**: `NaN === NaN` will return `false`. For this reason, you cannot use an expression like `x == NaN`. Instead, use `isNaN()`. A companion method is `isFinite()`, which will return true for anything but `NaN`, `Infinity`, or `-Infinity`.

`-0` compares equal to positive zero, even using strict equality testing: `-0 === 0` will return `true.` 

## Dates

Date handling is provided by methods on the core object `Date`.

## References

Flanagan, David. *Javascript: The Definitive Guide.* 6th ed, O'Reilly, 2011.
