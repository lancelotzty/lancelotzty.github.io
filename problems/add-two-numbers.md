## add-two-numbers

### idea
store the carry, add the value when either of the number node is not None

### approach
* #### procedure
  * add the first two nodes of the num lists
  * go through nodes when either of the num list is not empty
  * add the node to value when the num node is not None
  * **need to keep a reference of the start of the number, when updating the next node, need a seperate reference**
* #### analysis
  * time complexity --> O(n)
    * go throught the max length of the num list, O(1) in each iteration
  * sapce complexity --> O(1)

### code
``` python
def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        value = l1.val + l2.val
        l1 = l1.next
        l2 = l2.next
        count = 0
        if value >= 10:
            value -= 10
            count = 1
        result = ListNode(value)
        curr = result
        # stop when both are None
        while l1 is not None or l2 is not None:
            value = count
            if l1 is not None:
                value += l1.val
                l1 = l1.next
            if l2 is not None:
                value += l2.val
                l2 = l2.next
            count = 0
            if value >= 10:
                value -= 10
                count = 1
            curr.next = ListNode(value)
            curr = curr.next
        # add count
        if count == 1:
            curr.next = ListNode(count)
        return result
```
<br>

[back](../index.md)