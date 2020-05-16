## longest-substring-without-repeating-characters

### idea
sliding window with two pointers, store the chars occured in the window.

### approach
* #### procedure
  * create two pointer from start
  * maintain a set to store char within the range
  * update end when encountering new char
  * update start when encountering repeat
  * **tips**
    * use a for instead of while can avoid checking end exceeding range
    * use `end - start` instead of count to avoid calculation
* #### analysis
  * time complexity --> O(n)
    * as go through the string twice maximum
  * space complexity --> O(1)
    * only maintain 2 pointers and a maxLength

### code
``` python
def lengthOfLongestSubstring(self, s: str) -> int:
        size = len(s)
        start, end = 0, 0
        chars = set()
        maxL = 0
        for end in range(size):
            #update end till encounter repeat
            if s[end] not in chars:
                chars.add(s[end])
                continue
            #update max length
            count = end - start
            if maxL < count:
                maxL = count
            #update start till the repeat
            while s[start] != s[end]:
                chars.remove(s[start])
                start += 1
            # add 1 to skip the repeat
            start += 1
        count = size - start
        if maxL < count:
            maxL = count
        return maxL
```
<br>

[back](../index.md)