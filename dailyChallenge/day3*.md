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


## LC 59 spiral-matrix-ii
[LC Link](https://leetcode.com/problems/spiral-matrix-ii/description/)   
[Cousrse Link](https://programmercarl.com/0059.%E8%9E%BA%E6%97%8B%E7%9F%A9%E9%98%B5II.html#%E6%80%9D%E8%B7%AF)  
- follow the same range format: []

```python
    def generateMatrix(self, n: int) -> List[List[int]]:
        matrix = [[0]*n for _ in range(n)]

        num = 1

        # []
        top, bottom, left, right = 0, n-1, 0, n-1

        while left <= right and top <= bottom:

            # from left to right
            for i in range(left, right+1):
                matrix[top][i] = num
                num += 1
            top += 1

            # from top to bottom
            for j in range(top, bottom+1):
                matrix[j][right] = num
                num += 1
            right -= 1

            # from right to left
            for i in range(right, left - 1, -1):
                matrix[bottom][i] = num
                num += 1
            bottom -= 1

            # from bottom to top
            for j in range(bottom, top - 1, -1):
                matrix[j][left] = num
                num += 1
            left += 1

        return matrix
```
Time: **O(n^2)**   
Space: **O(n^2)**


## LC 303 squares-of-a-sorted-array
[LC Link](https://leetcode.com/problems/range-sum-query-immutable/description/)   
[Cousrse Link](https://labuladong.online/algo/data-structure/prefix-sum/)  
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
