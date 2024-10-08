# Day7 Hash Table Part 2

## LC 454 4sum-ii
[LC Link](https://leetcode.com/problems/4sum-ii/description/)   
[Cousrse Link](https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html)
- Different from typical 4 sums. Cannot use one element several times.
- Use one hash map to store possible sum of 2 arrays and save the time complexity from n^4 to n^2

```python
  def fourSumCount(self, nums1, nums2, nums3, nums4):
      # Use a dictionary to store the sums of elements from nums1 and nums2
      hashmap = dict()
      for n1 in nums1:
          for n2 in nums2:
              hashmap[n1+n2] = hashmap.get(n1+n2, 0) + 1
      
      # If -(n1+n2) exists in the sums of nums3 and nums4, add to the result
      count = 0
      for n3 in nums3:
          for n4 in nums4:
              key = -n3 - n4
              if key in hashmap:
                  count += hashmap[key]
      return count


```
Time: **O(n^2)**   
Space: **O(n^2)**


## LC 383 ransom-note
[LC Link](https://leetcode.com/problems/ransom-note/description/)   
[Cousrse Link](https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html)  
- Like LC 242 Valid Anagram, but 242 need reordered strA = str B. 383 only needs strB can be found in strA.

```python
        count = defaultdict(int)
        for char in magazine:
            count[char] += 1
            
        for s in ransomNote:
            if  count[s] == 0:
                return False
            count[s] -= 1
       
        return True
```
Time: **O(n)**   
Space: **O(1)**  - only lower case letter


## LC 15 3sum
[LC Link](https://leetcode.com/problems/3sum/description/)   
[Cousrse Link](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)  

- Callback of LC 1 Two Sun: cannot use two pointers since we need to return index while **two pointers need to sort first**

_Best Solution Award_
- Iterate while use two pointers to find out possible combinations
- Use **set** to de-dupelicate otherewise we need to prune to avoid dupes
```python
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums = sorted(nums)
        result = set()
        for i in range(len(nums)):
            l = i + 1
            r = len(nums) - 1
            target = 0 - nums[i]
            while l < r:
                if nums[l] + nums[r] == target:
                    result.add((nums[i], nums[l], nums[r]))
                    l += 1
                    r -= 1
                elif nums[l] + nums[r] < target:
                    l += 1
                else:
                    r -= 1
        return list(result)
```
Time: **O(n^2)**   
Space: **O(n)**


- Iterate and use Hash to save time 
- Be careful about pruning:
   - when num[i]>0
   - when check dupes for a, b
```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        nums.sort()
        # Find combinations where a + b + c = 0
        # a = nums[i], b = nums[j], c = -(a + b)
        for i in range(len(nums)):
            # After sorting, if the first element is greater than zero, it's impossible to find a valid triplet
            if nums[i] > 0:
                break
            if i > 0 and nums[i] == nums[i - 1]:  # Remove duplicate element a in the triplet
                continue
            d = {}
            for j in range(i + 1, len(nums)):
                if j > i + 2 and nums[j] == nums[j-1] == nums[j-2]:  # Remove duplicate element b in the triplet
                    continue
                c = 0 - (nums[i] + nums[j])
                if c in d:
                    result.append([nums[i], nums[j], c])
                    d.pop(c)  # Remove duplicate element c in the triplet
                else:
                    d[nums[j]] = j
        return result

```
Time: **O(n^2)**   
Space: **O(n)**



## [x]LC 18 4sum
[LC Link](https://leetcode.com/problems/4sum/description/)   
[Cousrse Link](https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html)  

- Add another layer of for loop: two nested for loops and two pointers
- This maynot be the best solution with so many de-dupes. We can de-deupe with set while sacrifice space
```python
      def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        result = []
        for i in range(n):
            if nums[i] > target and nums[i] > 0 and target > 0:# prune
                break
            if i > 0 and nums[i] == nums[i-1]:# de-dupe
                continue
            for j in range(i+1, n):
                if nums[i] + nums[j] > target and target > 0: #prune
                    break
                if j > i+1 and nums[j] == nums[j-1]: # de-dupe
                    continue
                left, right = j+1, n-1
                while left < right:
                    s = nums[i] + nums[j] + nums[left] + nums[right]
                    if s == target:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left+1]:
                            left += 1
                        while left < right and nums[right] == nums[right-1]:
                            right -= 1
                        left += 1
                        right -= 1
                    elif s < target:
                        left += 1
                    else:
                        right -= 1
        return result
```
Time: **O(n^3)**   
Space: **O(1)**


## Adds on
