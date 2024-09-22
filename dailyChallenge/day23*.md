# Day23 Backtracking Part2


## LC 39 combination-sum
[Link](https://leetcode.com/problems/combination-sum/description/)   
[Cousrse Link](https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html)    

- When do we need startIndex for combination problems?
    - when finding combinations from a single set, startIndex is necessary, like LC 77. Combinations and LC 216. Combination Sum III.
    - if the combinations are taken from multiple sets that do not affect each other, then startIndex is not needed like LC 17. Letter Combinations of a Phone Number
- !!How to dupe the values:
    - passing the same i in recursion as startIndex
- How to prune?
    -  firstly, sort the candidates and then check currrent sum > target 
```python
class Solution:

    def backtracking(self, candidates, target, total, startIndex, path, result):
        if total > target:
            return
        if total == target:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, total, i, path, result)  # 不用i+1了，表示可以重复读取当前的数
            total -= candidates[i]
            path.pop()

    def combinationSum(self, candidates, target):
        result = []
        self.backtracking(candidates, target, 0, 0, [], result)
        return result


# pruned version
class Solution:

    def backtracking(self, candidates, target, total, startIndex, path, result):
        if total == target:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            if total + candidates[i] > target:
                break
            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, total, i, path, result)
            total -= candidates[i]
            path.pop()

    def combinationSum(self, candidates, target):
        result = []
        candidates.sort()  # 需要排序
        self.backtracking(candidates, target, 0, 0, [], result)
        return result

```
Time: **O(n * 2^n)**     
Space: **O(target)** 

##  LC 40 combination-sum-ii
[Link](https://leetcode.com/problems/combination-sum-ii/description/)   
[Cousrse Link](https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html)
  
- Tricky part is:
    - have dupes in candidates but cannot return dupe results
    - elements can be repeated within the same combination but two combinations cannot be identical
- Understand what is Used and avoid dupes
    -  one is 'used' on the same branch(recursion), and the other is 'used' on the same level (for loop)
    -  **eliminate duplicates at the same level of the tree**
```python
class Solution:


    def backtracking(self, candidates, target, total, startIndex, path, result):
        if total == target:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            if i > startIndex and candidates[i] == candidates[i - 1]: # de-dupe 
                continue

            if total + candidates[i] > target:
                break

            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, total, i + 1, path, result)
            total -= candidates[i]
            path.pop()

    def combinationSum2(self, candidates, target):
        result = []
        candidates.sort()
        self.backtracking(candidates, target, 0, 0, [], result)
        return result
```
- Just another undestanding is using bool array called used
    - When used[i - 1] == true, it indicates that candidates[i - 1] has been used on the same branch.
    - When used[i - 1] == false, it indicates that candidates[i - 1] has been used on the same level <= the current candidate candidates[i] is being taken as a result of backtracking from candidates[i - 1]
```python
class Solution:


    def backtracking(self, candidates, target, total, startIndex, used, path, result):
        if total == target:
            result.append(path[:])
            return

        for i in range(startIndex, len(candidates)):
            # 对于相同的数字，只选择第一个未被使用的数字，跳过其他相同数字
            if i > startIndex and candidates[i] == candidates[i - 1] and not used[i - 1]:
                continue

            if total + candidates[i] > target:
                break

            total += candidates[i]
            path.append(candidates[i])
            used[i] = True
            self.backtracking(candidates, target, total, i + 1, used, path, result)
            used[i] = False
            total -= candidates[i]
            path.pop()

    def combinationSum2(self, candidates, target):
        used = [False] * len(candidates)
        result = []
        candidates.sort()
        self.backtracking(candidates, target, 0, 0, used, [], result)
        return result
```
Time: **O(n * 2^n)**     
Space: **O(n)** 


##  *LC 131 palindrome-partitioning
[Link](https://leetcode.com/problems/palindrome-partitioning/description/)   
[Cousrse Link](https://programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html)
- Key points:  
    - Cutting problems can be abstracted as combination problems.
    - How to simulate those cut lines => startIndex
    - How to terminate recursion in cutting problems => startIndex >= len(s)
    - How to extract substrings in the recursive loop => self.is_palindrome(s, start_index, i)
    - How to check for palindromes => two pointers / dp
```python
class Solution:

    def partition(self, s: str) -> List[List[str]]:
        '''
        递归用于纵向遍历
        for循环用于横向遍历
        当切割线迭代至字符串末尾，说明找到一种方法
        类似组合问题，为了不重复切割同一位置，需要start_index来做标记下一轮递归的起始位置(切割线)
        '''
        result = []
        self.backtracking(s, 0, [], result)
        return result

    def backtracking(self, s, start_index, path, result ):
        # Base Case
        if start_index == len(s):
            result.append(path[:])
            return
        
        # 单层递归逻辑
        for i in range(start_index, len(s)):
            # 此次比其他组合题目多了一步判断：
            # 判断被截取的这一段子串([start_index, i])是否为回文串
            if self.is_palindrome(s, start_index, i):
                path.append(s[start_index:i+1])
                self.backtracking(s, i+1, path, result)   # 递归纵向遍历：从下一处进行切割，判断其余是否仍为回文串
                path.pop()             # 回溯


    def is_palindrome(self, s: str, start: int, end: int) -> bool:
        i: int = start        
        j: int = end
        while i < j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True 
```
