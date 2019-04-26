---
title: 'JavaScript types: string'
---

JavaScript has no character type. Strings are primitive types, so they are immutable. They are zero-indexed, can be accessed using subscript notation `'foo'[0]` and have a `length` property provided by the `String` wrapper object. An empty string has a `length` of 0. Concatenation is performed by `+`. String literals can be continued across a line break in source code using `\`.

JavaScript strings are UTF-16 encoded, and each *code unit* is represented by a 16-bit pattern. For this reason, the `length` property of a string returns its length in code units, not characters; if your language uses two-unit characters, you're not going to get an accurate character length:

```js
> "🔥".length
2
```

The same applies to string indexing, using either `charAt()` or subscript notation:

```js
> "🔥💡".charAt(0)
"�"
> "🔥💡"[0]
"�"
```

The same applies to string-manipulation methods.

`for .. of`, a relatively recent feature, will operate on full characters, not code units:

```js
> for (const char of "🔥💡") console.log(char)
🔥
💡
```

Similarly, while `charCodeAt()` will return a code unit, not a full character code, you can get the latter with `codePointAt()`, introduced after problems with UTF-16 had been absorbed.

`String` object methods include Java-style `charAt()`; `indexOf()` and `lastIndexOf()`; `substring()` and `slice()`; `split()`; case conversion methods; and `search()`, `match()`, and `replace()`. Because strings are immutable, **mutator methods return a new string**.


## Sources

Flanagan, David. *Javascript: The Definitive Guide.* 6th ed, O'Reilly, 2011.

Haverbeke, Marijn. *Eloquent Javascript: A Modern Introduction to Programming.* <http://eloquentjavascript.net>.
