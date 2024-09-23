# Day25 Backtracking Part4


## LC 93 restore-ip-addresses
[Link](https://leetcode.com/problems/restore-ip-addresses/description/)   
[Cousrse Link](https://programmercarl.com/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.html)    

- Like 131 but add a point_sum for returning conditio
```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        result = []
        self.backtracking(s, 0, 0, "", result)
        return result

    def backtracking(self, s, start_index, point_num, current, result):
        if point_num == 3:  # 逗点数量为3时，分隔结束
            if self.is_valid(s, start_index, len(s) - 1):  # 判断第四段子字符串是否合法
                current += s[start_index:]  # 添加最后一段子字符串
                result.append(current)
            return

        for i in range(start_index, len(s)):
            if self.is_valid(s, start_index, i):  # 判断 [start_index, i] 这个区间的子串是否合法
                sub = s[start_index:i + 1]
                self.backtracking(s, i + 1, point_num + 1, current + sub + '.', result)
            else:
                break

    def is_valid(self, s, start, end):
        if start > end:
            return False
        if s[start] == '0' and start != end:  # 0开头的数字不合法
            return False
        num = 0
        for i in range(start, end + 1):
            if not s[i].isdigit():  # 遇到非数字字符不合法
                return False
            num = num * 10 + int(s[i])
            if num > 255:  # 如果大于255了不合法
                return False
        return True
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
