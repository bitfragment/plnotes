---
title: 'JavaScript objects: wrapper objects'
---

Primitive types are provided with wrapper objects that are created when you access a property or method of a primitive value, and discarded afterward.

Thus `'foo'.length` is temporarily wrapped with a `String` object, as if `new String()` had been called; `(1.234).toPrecision(3)` is temporarily wrapped with a `Number` object, as if `new Number()` had been called; and `(1 == 1).toString()` is temporarily wrapped with a `Boolean` object, as if `new Boolean()` had been called.

You can do this programmatically, with e.g., `s = new String('foo')`, but there is no good reason to do so.

The value and its wrapper object are non-strictly (`==`) equal and strictly (`===`) unequal. `typeof` will return `object` for the wrapper, and a primitive type designation otherwise.

```js
> 'foo' == new String('foo')
true
> 'foo' === new String('foo')
false
> typeof 'foo'
'string'
> typeof new String('foo')
'object'
```


## References

Flanagan, David. *Javascript: The Definitive Guide.* 6th ed, O'Reilly, 2011.
