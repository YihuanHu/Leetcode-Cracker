# Day 2 Sliding Window + Presum

## LC 209 minimum-size-subarray-sum
[LC Link](https://leetcode.com/problems/minimum-size-subarray-sum/description/)   
[Cousrse Link](https://programmercarl.com/0209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

- brute force: two loops => one for start [i in range(len)], another for end [j in (i, len)]
- Sliding window is kinda agianst this intuit by **iterating on the end point j**

```python
def minSubArrayLen(self, target: int, nums: List[int]) -> int:
    l , sum = 0, 0
    res = float('inf')
    for r in range(len(nums)):
        sum += nums[r]

        while sum >= target:
            length = r - l + 1
            res = min(res, length)
            sum -= nums[l] 
            l += 1

    return res if res != float('inf') else 0

```
Time: **O(n)**   <= each element is used twice (add and subtract). Basically **2n**   
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
