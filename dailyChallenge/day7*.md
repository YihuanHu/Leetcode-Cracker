# Day7 Hash Table Part 2
# Hash
- Sacrifice space to get O(1) query time => frequetly used when check the existance of a set
- Hash function: Maps input data to a fixed-size integer value (hash code) that represents the data, used to index into a hash table
- Hash Collision: When two different inputs produce the same hash code
  - Linear probing: Resolving collisions by sequentially searching for the next available slot in the hash table
  - Chaining: Resolving collisions by maintaining a list of all elements that hash to the same index
- Common data structures in Python:
  - `dict`: A mutable mapping of unique keys to values, allowing O(1) average-time complexity for lookups, inserts, and deletions.
    - `for key, value in my_dict.items():`
    - `for key in my_dict:`
    - `my_dict.pop('b')`
    - `my_dict.get('d', 0)`
  - `set`: An unordered collection of unique elements that supports O(1) average-time complexity for membership tests, insertions, and deletions. But running slower than array
    - `my_set.add(4)`
    - `my_set.remove(2)`
  - `array`: can be used for direct addressing when the range of the hash function is known and limited, creating a fixed-length array
    - `my_array.append(5)` 
    - `my_array[index]`  
    - `my_array.pop()`


## LC 242 valid-anagram
[LC Link](https://leetcode.com/problems/valid-anagram/)   
[Cousrse Link](https://programmercarl.com/0242.%E6%9C%89%E6%95%88%E7%9A%84%E5%AD%97%E6%AF%8D%E5%BC%82%E4%BD%8D%E8%AF%8D.html)


```python
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        dicS, dicT = {} , {}

        for i in range(len(s)):
            dicS[s[i]] = 1 + dicS.get(s[i],0)
            dicT[t[i]] = 1 + dicT.get(t[i],0)

        return dicS == dicT

```
Time: **O(n)**   
Space: **O(1)**


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
