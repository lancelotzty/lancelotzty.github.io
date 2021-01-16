## median-of-two-sorted-arrays
### idea1
Use binary search to find the k-th number in the two arrays.

### approach
* #### procedure
  * implement a function to find number of elements smaller than a value with binary search
  * implement a function to find the k-th element in the two arrays with binary search
  * find the median element in the two arrays

* #### analysis
  * the time complexity is O(log(m)log(n))
    * the find smaller function is O(log(n))
    * the find k-th element is O(log(m)log(n))
  * the space complexity is O(1)

### code
``` python
def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
   size1 = len(nums1)
   size2 = len(nums2)
   # if one of the list is empty
   if size1 == 0 or size2 == 0:
      l = nums1
      if size1 == 0:
            l = nums2
      size = len(l)
      median = size // 2
      if size % 2 == 0:
            return (l[median] + l[median - 1]) / 2
      return l[median]
   # if both are not empty
   median = (size1 + size2) // 2
   if median * 2 != size1 + size2:
      return self.findKth(nums1, nums2, median + 1)
   left = self.findKth(nums1, nums2, median)
   right = self.findKth(nums1, nums2, median + 1)
   return (right + left) / 2
   
      
      
def findKth(self, nums1, nums2, k):
   if len(nums1) < len(nums2):
      nums1, nums2 = nums2, nums1
   size = len(nums1)
   start = 0
   end = size - 1
   while start < end:
      mid = (start + end) // 2
      numSmaller = self.findNumSmaller(nums1[mid], nums2)
      if mid + numSmaller == k - 1:
            return nums1[mid]
      if mid + numSmaller > k - 1:
            end = mid
      else:
            start = mid + 1
   mid = start
   tmp = mid + self.findNumSmaller(nums1[mid], nums2)
   if tmp == k - 1:
      return nums1[mid]
   offset = k - mid - 2
   # check whether current mid is included
   if tmp > k - 1:
      offset += 1
   return nums2[offset]
   
   
# find the number smaller than value in a sorted array
def findNumSmaller(self, value, nums: List[int]) -> int:
   size = len(nums)
   if size == 1:
      return value > nums[0]
   start = 0
   end = size
   while start < end:
      mid = (start + end) // 2
      if value == nums[mid]:
            # check repeat
            if value == nums[mid - 1]:
               end = mid -1
               continue
            return mid
      if value < nums[mid]:
            end = mid
      else:
            start = mid + 1
   return start
```

### idea2
Use binary searh to find the two partations points in the two arraies.

### approach
* #### procedure
  * use binary search to find the partation `mid` of one of the array
  * find the another partation `another` of the another array
  * binary search the first array with the termination condition 
    * `nums1[mid - 1] <= nums2[another]`
    * `nums2[anotehr - 1] <= nums1[mid]`
  
* #### analysis
  * the time complexity is O(logn)
    * the binary seach on the short array is O(logn)
  * the space complexity is O(1)

### code
```python
def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
   size1 = len(nums1)
   size2 = len(nums2)
   isOdd = (size1 + size2) % 2 != 0
   # ensure nums1 smaller or equal to nums2
   if size1 > size2:
      nums1, nums2 = nums2, nums1
      size1, size2 = size2, size1
   start = 0
   end = size1
   while start <= end:
      mid = (start + end) // 2
      another = (size1 + size2 + 1) // 2 - mid
      # terminate condition
      if (mid == 0 or nums1[mid - 1] <= nums2[another]) and (mid == size1 or nums1[mid] >= nums2[another - 1]):
            if mid == 0:
               leftM = nums2[another - 1]
            elif another == 0:
               leftM = nums1[mid - 1]
            else:
               leftM = max(nums1[mid - 1], nums2[another - 1])
            if isOdd:
               return leftM
            if mid == size1:
               rightM = nums2[another]
            elif another == size2:
               rightM = nums1[mid]
            else:
               rightM = min(nums1[mid], nums2[another])
            return (leftM + rightM) / 2
      # mid is too large
      if mid > 0 and nums1[mid - 1] > nums2[another]:
            end = mid - 1
      else:
            start = mid + 1
   return 0
```
<br>

[back](../index.md)