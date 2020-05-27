## string-to-integer-atoi

### Idea
Use regex to filter out the valid string period

### To notice
all the int presented should be within the range of 32 bit signed integer

### Approach
* Use regex to filter out the valid string span
* apply the filter to remove whitespace, leading 0
* check whether it is within the valid range
* return with int()

### Analysis
* time complexity is O(n) where n is the length of the string
  * to apply regex
* space complexity is O(1)
  * only store the string
  
### Code
``` python
def myAtoi(self, str: str) -> int:
   # remove spaces
   import re
   pattern = '\A\s*[+,-]?\d+'
   num = re.search(pattern, str)
   if num:
      num = num.group()
      while num[0] == ' ':
            num = num[1:]
      sign = 1
      if num[0] == '-':
            num = num[1:]
            sign = -1
      elif num[0] == '+':
            num = num[1:]
      while len(num) > 0 and num[0] == '0':
            num = num[1:]
      if len(num) == 0:
            return 0
      if sign == -1:
            if len(num) > 10 or (len(num) == 10 and (int(num[:-1]) > 214748364 or (int(num[:-1]) == 214748364 and int(num[-1]) > 8))):
               return -2147483648
      else:
            if len(num) > 10 or (len(num) == 10 and (int(num[:-1]) > 214748364 or (int(num[:-1]) == 214748364 and int(num[-1]) > 7))):
               return 2147483647
      return sign * int(num)
   return 0
```

