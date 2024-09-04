# Day7 Hash Table Part 2
# Hash

## LC 454 4sum-ii
[LC Link](https://leetcode.com/problems/4sum-ii/description/)   
[Cousrse Link](https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html)
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


## LC 202 happy-number
[LC Link](https://leetcode.com/problems/happy-number/description/)   
[Cousrse Link](https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html#%E6%80%9D%E8%B7%AF)  
- Once there is a dupe, it starts an loop

```python
    def isHappy(self, n: int) -> bool:
        record = set()
        while n not in record:
            record.add(n)
            new_num = 0

            n_str = str(n)
            for i in n_str:
                new_num += int(i)**2
            if new_num == 1: return True
            print(new_num)
            n = new_num

        return False
```
Time: **O(n)**   
Space: **O(n)**


## LC 160 intersection-of-two-linked-lists
[LC Link](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)   
[Cousrse Link](https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html)  

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
