# Day3 Linked List
- defination for single LL
```python
class ListNode:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
```
|     | Insert/Delete Time Complexity | Query | Use Cases |
|-----|-------------------------|-------------------|----------|
| Array | O(n)                    | O(1)              | Fixed data size, frequent search, few insertions/deletions |
| LL | O(1)                    | O(n)              | Variable data size, frequent insertions/deletions, few searches |



## LC 203 /remove-linked-list-elements
[LC Link](https://leetcode.com/problems/remove-linked-list-elements/description/)   
[Cousrse Link](https://programmercarl.com/0203.%E7%A7%BB%E9%99%A4%E9%93%BE%E8%A1%A8%E5%85%83%E7%B4%A0.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

- Create an extra **dummy head** ListNode to handle the head edge case
- Manipulate of LL needs to think one step ahead
    - That's to say, if you want to update curr.val. You need to stand at curr.prev and delete the link by curr.prev.next
    - That's why check **curr.next is not null** is important to avoid null pointer

```python
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # dummy head in case to delete head
        dummy = ListNode(next = head) 

        curr= dummy
        while curr.next:
            if curr.next.val == val:
                curr.next = curr.next.next
            else: 
                curr = curr.next
            
        return dummy.next
```
Time: **O(n)**   
Space: **O(1)**


## LC 707 design-linked-list
[LC Link](https://leetcode.com/problems/design-linked-list/description/)   
[Cousrse Link](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html#%E6%80%9D%E8%B7%AF)  
- get familiar with LL manipulation

```python
  class MyLinkedList:

    def __init__(self):
        self.dummy = ListNode()
        self.size = 0
        

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        curr = self.dummy.next 
        for i in range(index):
            curr = curr.next
        return curr.val

    def addAtHead(self, val: int) -> None:
        self.dummy.next = ListNode(val, self.dummy.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        curr = self.dummy
        while curr.next:
            curr= curr.next
        curr.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return 
        curr = self.dummy
        for i in range(index):
            curr = curr.next
        curr.next = ListNode(val, next = curr.next)
        self.size += 1
        

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return 
        curr = self.dummy
        for i in range(index):
            curr = curr.next
        curr.next = curr.next.next
        self.size -= 1
```


## LC 206 reverse-linked-list
[LC Link](https://leetcode.com/problems/reverse-linked-list/description/)   
[Cousrse Link](https://programmercarl.com/0206.%E7%BF%BB%E8%BD%AC%E9%93%BE%E8%A1%A8.html)  
- **be careful about index P[i,j] =P[j+1] - P[i] where P[i] is the prefix sum of all the elements < i**


```python
    def __init__(self, nums: List[int]):
        self.preSum = [0] * (len(nums) + 1)
        
        for i in range(1, len(self.preSum)):
            self.preSum[i] = self.preSum[i - 1] + nums[i - 1]

    def sumRange(self, left: int, right: int) -> int:
        return self.preSum[right + 1] - self.preSum[left]
```
Time: **O(n)**   
Space: **O(n)**


## Adds on
- [ ] LC 304 range-sum-query-2d-immutable   [LC link](https://leetcode.com/problems/range-sum-query-2d-immutable/description/)
