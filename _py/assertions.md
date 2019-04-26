---
title: 'Python assertions'
---

You can include an optional error message in an `assert` statement:

```py
>>> assert 1 + 1 == 2, "Uh oh"
```

There is a gotcha here! *`assert` is a statement, not a function.* **Do not write it in parenthesized form! To do so is to evaluate a tuple, and since a tuple is truthy, it will always evaluate to true.** Python will provide a warning in this case:

```py
>>> assert(1 + 1 == 2, "Uh oh")
<stdin>:1: SyntaxWarning: assertion is always true, perhaps remove parentheses?
```


## Sources

Dan Bader, *Python Tricks: The Book*, 2017.
