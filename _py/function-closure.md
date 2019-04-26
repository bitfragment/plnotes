---
title: "Python function closure"
---

```py
def ctt_from_to(begin, end):
    def count():
        return list(range(begin, end + 1))
    return count


ctt_3_10 = ctt_from_to(3, 10)

assert ctt_3_10() == [3, 4, 5, 6, 7, 8, 9, 10]
```
