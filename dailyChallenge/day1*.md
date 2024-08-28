# Day 1 Binary Array

## LC 704 binary-search
[LC Link](https://leetcode.com/problems/binary-search/description/)   
[Cousrse Link](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html)

- **sort and unique array**
- Logic chain is:   
choose range type (right close/open)    
=> can we take the right (last element / null)   
=> how to update right(right = mid -1 / mid) when mid > target ?

```python
def search(self, nums: List[int], target: int) -> int:
    # left righ close []
    left , right = 0, len(nums)-1

    while left <= right: # !!! 
         mid = left + (right - left ) //2  # avoid overflow (right + left ) //2
         if nums[mid] < target:
           left = mid + 1
         elif nums[mid] > target:
            right = mid - 1 # !!!
         else:
            return mid

    return -1

```
Time: **O(log n)**   
Space: **O(1)**


## LC 27 remove-element
[LC Link](https://leetcode.com/problems/remove-element/description/)   
[Cousrse Link](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html)  
- ** Two pointers: Slow fast **
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
- ** Two pointers: Opposite direaction **
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
