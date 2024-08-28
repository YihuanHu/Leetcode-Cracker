# Day 1 Binary Array

## LC 704 binary-search
[LC Link](https://leetcode.com/problems/binary-search/description/)   
[Cousrse Link](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html)

- **sort and unique array**
- Logic chain is:   
choose range type (right close/open)    
=> can we take the right (last element / null)   
=> how to handle update right(right = mid -1 / mid) when mid > target ?

```python
class Solution:
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

## LC 27 remove-element
[Link]([https://leetcode.com/problems/binary-search/description/](https://leetcode.com/problems/remove-element/description/))


```python
class Solution:
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

## LC 977
