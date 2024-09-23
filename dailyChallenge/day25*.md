# Day25 Backtracking Part4


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

##  LC 78 subsets
[Link](https://leetcode.com/problems/subsets/description/)   
[Cousrse Link](https://programmercarl.com/0078.%E5%AD%90%E9%9B%86.html)
  
- Collecting all the results for each node
- While combination / splitting only collects the leaf node
```python
class Solution:
    def subsets(self, nums):
        result = []
        path = []
        self.backtracking(nums, 0, path, result)
        return result

    def backtracking(self, nums, startIndex, path, result):
        result.append(path[:])  # 收集子集，要放在终止添加的上面，否则会漏掉自己
        # if startIndex >= len(nums):  # 终止条件可以不加
        #     return
        for i in range(startIndex, len(nums)):
            path.append(nums[i])
            self.backtracking(nums, i + 1, path, result)
            path.pop()
```

Time: **O(n * 2^n)**     
Space: **O(n)** 


##  LC 90 subsets-ii
[Link](https://leetcode.com/problems/subsets-ii/)   
[Cousrse Link](https://programmercarl.com/0090.%E5%AD%90%E9%9B%86II.html)
- Similar to 78 but it has dupes
- **nums need to be sorted for de-dupe**
```python
# use startIndex for de-dupe
class Solution:
    def subsetsWithDup(self, nums):
        result = []
        path = []
        nums.sort()  # 去重需要排序
        self.backtracking(nums, 0, path, result)
        return result

    def backtracking(self, nums, startIndex, path, result):
        result.append(path[:])  # 收集子集
        for i in range(startIndex, len(nums)):
            # 而我们要对同一树层使用过的元素进行跳过
            if i > startIndex and nums[i] == nums[i - 1]:
                continue
            path.append(nums[i])
            self.backtracking(nums, i + 1, path, result)
            path.pop()

# use USED bool for de-dupe
class Solution:
    def subsetsWithDup(self, nums):
        result = []
        path = []
        used = [False] * len(nums)
        nums.sort()  # 去重需要排序
        self.backtracking(nums, 0, used, path, result)
        return result

    def backtracking(self, nums, startIndex, used, path, result):
        result.append(path[:])  # 收集子集
        for i in range(startIndex, len(nums)):
            # used[i - 1] == True，说明同一树枝 nums[i - 1] 使用过
            # used[i - 1] == False，说明同一树层 nums[i - 1] 使用过
            # 而我们要对同一树层使用过的元素进行跳过
            if i > 0 and nums[i] == nums[i - 1] and not used[i - 1]:
                continue
            path.append(nums[i])
            used[i] = True
            self.backtracking(nums, i + 1, used, path, result)
            used[i] = False
            path.pop()
```
