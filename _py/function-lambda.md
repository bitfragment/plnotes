---
title: "Python lambdas"
---

Lambdas, standard syntax:

```py
f = lambda x: x
assert f(1) == 1
```

Can't think of a good reason to do so, but I'd never realized that one can
omit the argument:

```py
f = lambda : 1
assert f() == 1
```
