---
title: 'JavaScript types: overview'
---

There are two categories of types: *primitive* types and *object* (sometimes also called *reference*) types. There are also the special types `null` and `undefined`.

One can also classify JavaScript types as mutable types and immutable types. Object types are mutable; primitive types (their wrapper objects notwithstanding) are immutable, as are `null` and `undefined`.

Primitive types are compared by *value*; object types, by *reference*.

Every value except `null` and `undefined` has a `toString()` method. Usually this value will be identical to the value returned by explicit coercion with `String()`.


## Primitive types

Numbers, strings, and booleans. There is no character type. 


## Object types

Any value that is not `null`, `undefined`, or a primitive type is an object type. Core object types are `Array`, `Function`, `Date`, `RegExp`, and `Error`.


## `null` and `undefined`

`null` is a *value indicating the absence of a value*, while `undefined` is a *value indicating that a value has not been assigned*. "The undefined value represents a deeper kind of absence" (Flanagan, *Javascript: The Definitive Guide*). 

Querying an array element or object property that does not exist will return `undefined`. Functions for which a return value has not been defined return `undefined`. A function parameters for which no argument is provided, when the function is invoked, is `undefined.`

`null` and `undefined` are non-strictly (`==`) equal, but strictly (`===`) unequal. They are both falsy. Neither has any methods.

`typeof null` returns `'object'`, while `typeof undefined` returns `'undefined'`. The meaning of this difference is unclear.


## References

Flanagan, David. *Javascript: The Definitive Guide.* 6th ed, O'Reilly, 2011.
