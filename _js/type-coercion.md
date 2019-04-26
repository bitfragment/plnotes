---
title: 'JavaScript type coercion'
---

## Implicit coercion

"JavaScript is very flexible about the types of values it requires... when JavaScript expects a boolean value, you may supply a value of any type, and JavaScript will convert it as needed... if JavaScript wants a string, it will convert whatever value you give it to a string. If JavaScript wants a number, it will try to convert the value you give it to a number (or to `NaN` if it cannot perform a meaningful conversion)" (Flanagan, *Javascript: The Definitive Guide*).

Thus `10 + ' apples'` is evaluated as `'10 apples'`; `'7' * '4'` is evaluated as 28; `'7' + '3'` is evaluated as `'73'` (because the `+` operator is overloaded for string concatenation); and `1 - 'x'` is evaluated as `NaN`.

`true` will be numerically coerced to 1, and `false`, `""`, and `[]` to 0.

`+`, `-` and `!` perform implicit type coercion: `1 + ""` is evaluated as "1", `+"1"` is evaluated as `1`, `"1" - 0` is evaluated as `1`, and `!!0` is evaluated as `false`.

Coercion plays a part in non-strict (`==`) equality: thus `null == undefined`, `"0" == 0`, `0 == false`, and `"0" == false` will all return true, while their strict (`===`) counterparts will all return false.


## Explicit coercion

Use the constructors `Number()`, `Boolean()`, and `String()` *without* the `new` keyword.

Used this way, the `Object` constructor, will perform implicit coercion: thus `Object(1)` evaluates to `[Number: 1]` (that is, a new `Number` object with the value 1).


## Explicit numeric coercion

Summary:

- Global function `parseInt()`
- Global function `parseFloat()`
- `Number()` constructor
- `Number` method `toString()`
- `Number` method `toPrecision()`
- `Number` method `toFixed()`
- `Number` method `toExponential()`

The global function `parseInt()` performs implicit conversion as well: `parseInt('1.23')` evaluates to `1`. The global function `parseFloat()` does not: `parseFloat('1')` evaluates to `1` (rather than 1.0), and `parseFloat('1.23'` evaluates to `1.23`. `

`parseFloat()` also accepts integers. `parseInt()` accepts an optional radix (base) argument from 2–36. `parseInt()` will parse a hex literal (e.g., `0x555555`). Both functions strip leading whitespace, parse whatever they can, and ignore the rest: thus `parseInt(' 123foo ')` is evaluated as `123`. If the first character of the string is non-numeric, it will be evaluated as `NaN`, so you can use `isNaN(parseInt())` for testing.

`Number()` will attempt to parse a string as a base 10 integer or floating-point value. 

The `toString()` method of `Number` accepts an optional radix (base) argument: `(10).toString(2)` returns `'1010'`; `(10).toString(16)` returns `'a'`.

`Number` also provides three other string conversion methods:

-  `toPrecision()`, taking an argument specifying the number of digits: `(1.2345).toPrecision(3)` ⟹ `'1.23'`
- `toFixed()`, taking an argument specifying the number of digits after the decimal point: `(1.2345).toPrecision(3)` ⟹ `'1.234'`
- `toExponential()`, taking an argument specifying the number of digits after the decimal point: `(1000000.2345).toExponential(2)` ⟹ `'1.00e+6'`


## Explicit object conversion

All objects inherit `toString()` and `valueOf()`. 

Some core object types define `toString()` in uninformative ways: `({}).toString()` evaluates to `'[object Object]'`. Others are more informative: `[1, 2, 3].toString()` evaluates to `'1,2,3'`. 

Called on an object type, `valueOf()` will simply return the object itself. But `Number(1).valueOf()` will return `1`, `String('foo').valueOf()` will return `'foo'`, and so on.

`new Date(2018,1,1).toString()` returns the human-readable `'Thu Feb 01 2018 00:00:00 GMT+0300 (+03)'`. `new Date(2018,1,1).valueOf()` returns `1517432400000` (ms since 1/1/1970).

`new RegExp(/foo/i).toString()` returns `'/foo/i'` (a string); `new RegExp(/foo/i).valueOf()` returns `/foo/i` (a regexp literal).

When converting an object to a string, `toString()` and `valueOf()` will be called in that order. If either returns a primitive value, it will be converted to a string. If neither method exists, a `TypeError` will be thrown.

When converting an object to a number, `valueOf()` and `toString()` will be called in that order. If either returns a primitive value, it will be converted to a number. If neither method exists, a `TypeError` will be thrown.


## References

Flanagan, David. *Javascript: The Definitive Guide.* 6th ed, O'Reilly, 2011.
