---
title: "Python types: numeric types"
---

Numeric values are immutable, and like everything else in Python, they are
represented by objects. To assign a new numeric value to a name is not to
"update" that value; it is to create a new object representing the new value
and to reassign the reference to the name. If you want to delete a reference to a number object, you need to use `del` to do it.

**Contents**
* toc
{:toc}


## Integer types

The **Boolean** integer type can have the constant value `True` or `False`, which are built-in names assigned to a subclass of the `int` type. It behaves like an integer value zero or one if used in a numeric context, but prints as `True` or `False` (by redefining its `str` and `repr` formats). All Python objects have a Boolean `True` or `False` value.

Python 2 unified standard sized (previously, architecture-dependent 32 or 64-bit) and unsized long integers. Since then, the **standard** or "plain" integer is unsized and does not overflow, and no special notation is needed. Binary is represented using the prefix `0b`, octal with `0o`, and hexadecimal with `0x`.

## Other numeric types

Double-precision floating point real numbers are implemented as C doubles. The actual degree of precision is architecture- and interpreter-dependent (that is, dependent on the C compiler that built the CPython interpreter). Scientific notation is available using the `e` suffix.

Decimal floating point numbers are available from the `Decimal` class in the `decimal` module.

Complex numbers use the format *real+imaginary`j`* (e.g., `12.34+1j`). You can access the attributes `n.real`, `n.imag`, and `n.conjugate()`.


## Integer type functions (available for integers only)

`bin()`, `oct()`, and `hex()` return string representations. `ord()` and `chr()` convert ASCII characters to integer values and integer values to ASCII character strings, respectively.


## Other numeric type functions (available for all numeric types)

`int()`, `float()`, `complex()`, and `bool()` perform two functions: (1) they convert from one numeric type to another; (2) they return the numeric value of a string. `int()` can take a base parameter for string conversions only. `complex()` can take separate parameters for the real and imaginary portions, for numeric conversions only.

These functions are *factory functions*: they create a new instance of a class.

`abs()` returns absolute value. `divmod()` returns the tuple (quotient, remainder). `pow()` can take an optional third parameter (a modulus argument, for applications in cryptography). `round()` takes an optional parameter, a specified number of decimal places. These arithmetic functions are built in; other functions and tools (e.g., `pi`, `e`, sqrt()) are available from the `math` module, which provides most of the functions in the C standard library `math`. Random number functions are available from the `random` module.

`int()` *truncates*; `floor()` rounds to the *next smaller* integer; `round()` rounds to the *nearest* integer.


## Sources

Chun, Wesley J. *Core Python Programming.* 2nd ed, Prentice Hall, 2007. Section 4.6, "Standard Type Built-in Functions"; Chapter 5, "Numbers."

Lutz, Mark. *Learning Python.* Third ed., O'Reilly, 2008. Chapter 5, "Numbers."
