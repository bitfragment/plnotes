---
title: 'Python syntax: pattern matching'
---

The Python string object **does not support pattern matching.** For this you need the module `re`, which implements Perl-style regular expressions.

Following Chun *Core Python Applications Programming,* we distinguish here between *searching* and *matching*.

## Syntax demo

```py
>>> import re
>>> re.compile('foo').search('foobar').group()
'foo'
>>> re.compile('foo').match('foobar').group()
'foo'
>>> x = re.compile('bar')
>>> m = x.match('foobar')
>>> if m is not None:
        m.group()

```

## Introduction

Best practice is to *compile* regular expression patterns explicitly, using `re.compile(pattern, flags=0)`. With a compiled pattern, *searching* is performed using `search(string, flags=0)` and *matching* is performed using `match(string, flags=0)` as *methods* on the compiled regexp object.

Otherwise, using them as functions, `search(pattern, string, flags=0)` and `match(pattern, string, flags=0)`. (`purge()` will purge the cache of *implicitly* compiled patterns.)

By default, `match()` matches *only at the beginning of the string*. By default, `search()` matches *anywhere in the string.*

`findall()` returns a list of non-overlapping matches; `finditer()` returns an iterator that returns a match object for each match.

## Match object methods

Both search and match operations return *match objects.* Methods on the match object include `group()`, which returns the entire match or a subgroup *num* passed as an argument; `groups()`, which returns all matching groups in a tuple, and `groupdict()`, which returns a dict containing matching subgroups.

## Transformations

`sub()` **replaces** occurrences of a pattern; `subn()` also returns the number of substitutions made.  `split(pattern, string, max=0)` splits the search string using the pattern as a delimiter and returns a list of successful matches up to `max` successful matches.


## Sources

Chun, Wesley. *Core Python Applications Programming.* 3rd ed, Prentice Hall, 2012.
