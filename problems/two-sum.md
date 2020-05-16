## problem two sum     

### approach
* #### procedure
  * create a dictionary to store the minus
  * return index when encountering the minus value
* #### analysis
  * time complexity --> O(n)
    * go through dict, each iteration O(1)
  * space complexity --> O(n)
    * dict with size O(n)

### code
``` python
def twoSum(self, nums: List[int], target: int) -> List[int]:
        minus = {}
        for i in range(len(nums)):
            curr = target - nums[i]
            if curr in minus:
                return [minus[curr], i]
            minus.update({nums[i]: i})
        return []
```
<br>

[back](../index.md)