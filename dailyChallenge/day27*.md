# Day27 Greedy Algo Part1

## LC 491 non-decreasing-subsequences
[Link](https://leetcode.com/problems/non-decreasing-subsequences/description/)   
[Cousrse Link](https://github.com/YihuanHu/Leetcode-Cracker/blob/main/dailyChallenge/day25*.md)    

- Different de-dupe logic:
    - Since the order is fixed we cannot sort and use startIndex to de-dupe
    - Use set for used numbers 
```python
class Solution:
    def findSubsequences(self, nums):
        result = []
        path = []
        self.backtracking(nums, 0, path, result)
        return result
    
    def backtracking(self, nums, startIndex, path, result):
        if len(path) > 1:
            result.append(path[:])  # 注意要使用切片将当前路径的副本加入结果集
            # 注意这里不要加return，要取树上的节点
        
        uset = set()  # 使用集合对本层元素进行去重
        for i in range(startIndex, len(nums)):
            if (path and nums[i] < path[-1]) or nums[i] in uset:
                continue
            
            uset.add(nums[i])  # 记录这个元素在本层用过了，本层后面不能再用了
            path.append(nums[i])
            self.backtracking(nums, i + 1, path, result)
            path.pop()
```
Time: **O(n * 2^n)**     
Space: **O(target)** 

##  LC 46 permutations
[Link](https://leetcode.com/problems/permutations/)   
[Cousrse Link](https://programmercarl.com/0046.%E5%85%A8%E6%8E%92%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
  
- Each level starts searching from 0 instead of startIndex since order matters 
- We need a used array to keep track of which elements are already included in the path.
```python
class Solution:
    def permute(self, nums):
        result = []
        self.backtracking(nums, [], [False] * len(nums), result)
        return result

    def backtracking(self, nums, path, used, result):
        if len(path) == len(nums):
            result.append(path[:])
            return
        for i in range(len(nums)):
            if used[i]:
                continue
            used[i] = True
            path.append(nums[i])
            self.backtracking(nums, path, used, result)
            path.pop()
            used[i] = False
```
Time: **O(n!)**     
Space: **O(n)** 


##  LC 47 permutations-ii
[Link](https://leetcode.cn/problems/permutations-ii/)   
[Cousrse Link](https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html)    
- **combination and permutation problems collect results at the leaf nodes of a tree structure, while the subset problem gathers results from all nodes on the tree.**
- Both used[i - 1] == true/false can be used
```python
class Solution:
    def permuteUnique(self, nums):
        nums.sort()  # 排序
        result = []
        self.backtracking(nums, [], [False] * len(nums), result)
        return result

    def backtracking(self, nums, path, used, result):
        if len(path) == len(nums):
            result.append(path[:])
            return
        for i in range(len(nums)):
            // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过 more effecient
            // 如果同一树层nums[i - 1]使用过则直接跳过
            if (i > 0 and nums[i] == nums[i - 1] and not used[i - 1]) or used[i]:  # de-dupe
                continue
            used[i] = True
            path.append(nums[i])
            self.backtracking(nums, path, used, result)
            path.pop()
            used[i] = False

```
Time: **O(n! * n )**     
Space: **O(n)** 

## Adds on
- [ ] 332, 51 , 37 for harder backtracking problems
- summary [Link](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E6%80%BB%E7%BB%93.html#%E7%BB%84%E5%90%88%E9%97%AE%E9%A2%98)
    - the power of backtracking: using recursion to control the number of nested for loops

