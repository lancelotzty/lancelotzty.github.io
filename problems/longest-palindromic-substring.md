## longest-palindromic-substring

### idea
there are three approaches explaned here
* with DP
  * maintain a table to store whether there is a palindrome from i to j
  * iterate all length and check palindrome base on table
* with smart expansion
  * there are intotal 2n - 1 centers (including the even centers which are two chars)
  * expand each of the center and check palindrome
* manacher's algo
  * maintain a expansion array
  * skip some of the center base on the expansion before
  * [manachers](../algos/manachers.pdf)

### approaches
* simple implementation following the idea

### analysis
* DP
  * time complexity is `O(n^2)`
    * the possible length are from `1` to `n`
    * in each length k need to go through the string from index `0` to `n - k - 1` and from the table check in `O(1)` time
  * space complexity is `O(n^2)`
    * maintains a `n*n` table store all possible start end combinations
* smart brute force
  * time complexity is `O(n^2)`
    * in total `2n - 1` centers
    * for each center expand in `O(n)` as check all the length
  * space complexity is `O(1)`
    * as no need to store any thing, only stores some pointers
* manacher's algo
  * time complexity is `O(n)`
  * spaec complexity is `O(n)`

### code 

#### DP
``` python
def longestPalindrome(self, s: str) -> str:
   size = len(s)
   if size <= 1:
      return s
   table = [[0 for i in range(size + 1)] for i in range(size + 1)]
   maxs = s[0]
   for i in range(size):
      table[i][i] = 1
   for i in range(size - 1):
      if s[i] == s[i + 1]:
            table[i][i + 1] = 1
            maxs = s[i: i + 2]
   maxL = 2

   for k in range(3, size + 1):
      if k > maxL + 2:
            break
      for i in range(1, size - k + 2):
            if table[i][i + k - 3] == 1 and s[i - 1] == s[i + k - 2]:
               table[i - 1][i + k - 2] = 1
               maxL = k
               maxs = s[i - 1: i + k - 1]
   return maxs
```

#### Manacher's algo
``` python
def longestPalindrome(self, s: str) -> str:
   # with manacher's algo
   n = len(s)
   right, center = 2, 1
   p = [0 for _ in range(2 * n + 1)]
   maxExpansion = 1
   maxCenter = 1
   # condition when there is no palindrome longer
   while maxExpansion + center < 2 * n + 1:
      i = p[center] + 1
      # expand the center from right
      while center - i >= 0 and center + i < 2*n+1 and ((center - i) % 2 == 0 or s[(center - i) // 2] == s[(center + i) // 2]):
            p[center] += 1
            i += 1
      # update max
      if p[center] > maxExpansion:
            maxExpansion = p[center]
            maxCenter = center
      # update right
      if center + p[center] > right:
            right = center + p[center]
      # find the newCenter
      i = 1
      newCenter = center + i
      while newCenter < right:
            mirrorCenter = center - i
            if p[mirrorCenter] + newCenter < right:
               p[newCenter] = p[mirrorCenter]
               newCenter  += 1
               i += 1
               continue
            p[newCenter] = right - newCenter
            break
      center = newCenter
   l = (maxCenter - maxExpansion) // 2
   r = (maxCenter + maxExpansion) // 2
   return s[l:r]
```
