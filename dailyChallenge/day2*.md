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


## LC 27 remove-element
[LC Link](https://leetcode.com/problems/remove-element/description/)   
[Cousrse Link](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html)  
- **Two pointers: Slow fast**
- Slow: store all the values satisfy requirements
- Fast: iteration 

```python
    def removeElement(self, nums: List[int], val: int) -> int:
         slow, fast = 0, 0
         size = len(nums)
         while fast < size:  # exclude equal since when fast might = size
            if nums[fast] == val:
                fast += 1
            elif nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
                fast += 1
         return slow
```
Time: **O(n)**   
Space: **O(1)**


## LC 977 squares-of-a-sorted-array
[LC Link](https://leetcode.com/problems/squares-of-a-sorted-array/description/)   
[Cousrse Link](https://programmercarl.com/0977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.html)  
- **Two pointers: Opposite direaction**
- starting at the start and end to look for largest square values
- Tips: range function in python is [) and we can start backward for non-descending order

```python
def sortedSquares(self, nums: List[int]) -> List[int]:
    n = len(nums)
    l , r = 0, len(nums) - 1
    res = [0] * n

    for i in range(n-1, -1, -1):
        if abs(nums[l]) < abs(nums[r]):
            res[i] = nums[r] * nums[r]
            r -= 1
        else:
            res[i] = nums[l] * nums[l]
            l += 1

    return res
```
Time: **O(n)**   
Space: **O(n)**


## Adds on
- [ ] review LC 27 with brute force & 2 pointers
