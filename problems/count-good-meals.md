## count good meal

### idea
As the sum need to be sum of power of 2, consider two numbers a, b, which a + b is sum of power of 2 named c. If b >= a, then c is the least power of 2 that is larger than b. Thus, only need to count the number of pairs, via getting the least larger power of two.

### approach
* #### procedure
  * find the least power of two which is larger.
  * if the difference is in the values, count the numebrs of matching.
    * if the difference is equal to the number, indicating the number is a power of two, thus, need to add the matchings with 0s.
    * for other values, just calculating the multiplication.
* #### analysis
  * time complexity --> O(n)
    * go through dict once, finding the least larger is O(1) can use bitwise to optimise
  * space complexity --> O(1)
    * input with size O(n)

### code
``` python
def countPairs(self, deliciousness: List[int]) -> int:
        from collections import Counter
        count = Counter(deliciousness)
        targets = [2 ** i for i in range(22)]
        get_upper = lambda x : 2 ** int(math.log2(x) + 1) if i > 0 else -1
        # can be optimise with lambda x: int('1' + '0' * len('{:b}'.format(x)), 2) if x > 0 else -1
        num = 0
        taken = set()
        for i in count.keys():
            diff = get_upper(i) - i
            if diff == i:
                num += count[i] * (count[i] - 1) // 2 + count[i] * count[0]
            elif diff in count.keys():
                num += count[i] * count[diff]    
        return num % (10 ** 9 + 7)
```
<br>

[back](../index.md)