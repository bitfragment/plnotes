---
title: 'Python types: collection types'
---

## Built-in collection types

Python's **collection types** include [sequence]({{site.baseurl}}/py/type-collection-sequence/) (string, tuple, list) and [mapping]({{site.baseurl}}/py/type-collection-mapping/) types (dictonaries).

They also include **sets**.


## Collection types in `collections`

### Deque

A Python deque can be of fixed size or unbounded. You can append to either end
and pop from either end. Appending or popping has O(1) complexity, versus O(N)
for a list.

Using a fixed-size deque to track recent history:

```py
>>> from collections import deque
>>> hist = deque(maxlen=3)
>>> for x in [1, 2, 3, 4, 5]:
    hist.append(x)
>>> hist
deque([3, 4, 5], maxlen=3)
```
