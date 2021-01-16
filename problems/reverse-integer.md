## reverse-integer

### Idea
simply pop the digit and revers

### To Notice
as python does not have int limit, need to check limit to ensure no overflow in requirement

### Approach
when poping the digit, check the accumulated result if it is larger than `2^31 // 10` or equal but the new digit larger than 8, return 0 directly

### Code
``` python
def reverse(self, x: int) -> int:
   # with rescrit that no larger int and no overflow
   limit = 2 ** 31 // 10
   if x == 0:
      return 0
   while x % 10 == 0:
      x //= 10
   sign = 1
   if x < 0:
      x = -x
      sign = -1
   result = 0
   while x != 0:
      tmp = x % 10
      if result > limit or result == limit and tmp > 8:
            return 0
      result = result * 10 + tmp
      x = x // 10
   return sign * result
```
<br>

[back](../index.md)