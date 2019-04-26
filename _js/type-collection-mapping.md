---
title: 'JavaScript types: collection types: mappings'
---

A mapping type is a collection that stores objects by key, rather than by position. **Mapping types maintain no reliable item order.**

Because JavaScript objects derive from `Object.prototype`, **you should not use a JavaScript object as a mapping type** (dictionary hash map, etc.). Instead, use `Map()`, which included `set()`, `get()`, and `has()`:

```js
> let m = new Map()
undefined
> m.set('foo', 1)
Map { 'foo' => 1 }
> m.set('bar', 2)
Map { 'foo' => 1, 'bar' => 2 }
> m.get('foo')
1
> m.get('bar')
2
> m.has('foo')
true
> m.has('qux')
false
```


## Sources

Haverbeke, Marijn. *Eloquent Javascript: A Modern Introduction to Programming.* <http://eloquentjavascript.net>.
