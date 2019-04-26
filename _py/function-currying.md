---
title: "Python partial function application"
---

Partial function application (currying), via `functools`:

```py
from functools import partial

# `int()` takes the argument `base=8`
oct2dec = partial(int, base=8)

assert oct2dec('173') == 123
```
