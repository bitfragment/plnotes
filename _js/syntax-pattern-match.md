---
title: 'JavaScript syntax: pattern matching'
---

JavaScript regular expressions can be written as literals: `var x = /foo/i`. They can also be constructed with the `RegExp()` object constructor: `var y = new RegExp(/foo/i)`. 

`RegExp` object methods include `test()`: `/foo/i.test('FOO')` will return true. `String` object methods can also accept regexps: `'foo'.search('o')` will return 1, the index of the first matching, while `'foo'.match('o')` will return an array of matches.


## Sources

Flanagan, David. *Javascript: The Definitive Guide.* 6th ed, O'Reilly, 2011.
