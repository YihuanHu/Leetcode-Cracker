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


##  LC 17 letter-combinations-of-a-phone-number
[Link](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)   
[Cousrse Link](https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html)
  
- think about edge cases of handling "0"
- how to handle when the set is diff
```python
class Solution:
    def __init__(self):
        self.letterMap = [
            "",     # 0
            "",     # 1
            "abc",  # 2
            "def",  # 3
            "ghi",  # 4
            "jkl",  # 5
            "mno",  # 6
            "pqrs", # 7
            "tuv",  # 8
            "wxyz"  # 9
        ]
        self.result = []
        self.s = ""
    
    def backtracking(self, digits, index):
        if index == len(digits):
            self.result.append(self.s)
            return
        digit = int(digits[index])    # 将索引处的数字转换为整数
        letters = self.letterMap[digit]    # 获取对应的字符集
        for i in range(len(letters)):
            self.s += letters[i]    # 处理字符
            self.backtracking(digits, index + 1)    # 递归调用，注意索引加1，处理下一个数字
            self.s = self.s[:-1]    # 回溯，删除最后添加的字符
    
    def letterCombinations(self, digits):
        if len(digits) == 0:
            return self.result
        self.backtracking(digits, 0)
        return self.result
```
