# Day7 Hash Table Part 2
# Hash

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


## LC 160 intersection-of-two-linked-lists
[LC Link](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)   
[Cousrse Link](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html)  

- Comparision why use array/set/dict
```python
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```
Time: **O(n+m)**   
Space: **O(1)**


## LC 1 two-sum
[LC Link](https://leetcode.com/problems/two-sum/)   
[Cousrse Link](https://programmercarl.com/0001.%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C.html)  

- why hash
- why dict/map
- what does map stores
- what is the key/value here
```python
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        seen = {}
        for i,value in enumerate(nums):
            remain = target - value
            if remain in seen:
                return [seen[remain],i]
            seen[value] = i
        return []
```
Time: **O(n)**   
Space: **O(1)**


## Adds on
