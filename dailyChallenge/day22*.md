# Day22 Backtracking Part1

## Backtracking 
- **Backtracking is a byproduct of recursion**; whenever there is recursion, backtracking will be present
- Backtracking fundamentally involves **exhaustive search**, exploring all possibilities to find the desired solution. While pruning techniques can enhance efficiency
    - Go vertically using recursion till hit the leaf and then backtrack pop,  go vertially using for loop
- Problems solved by backtracking can all be abstracted into a tree structure.
    - This is because backtracking involves recursively searching for subsets within a set.
    - **The size of the set determines the width of the tree**
    - **The depth of the recursion defines the depth of the tree**
- Sample
    - Returning & Parameter: usually return null & flexible parameters
    - End condition: not sure
    - Process:
        - A for loop can be understood as a horizontal traversal
        - backtracking (recursion) represents a vertical traversal.
        - Leaf node would be one solution
```pyrhon
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```
- Common questions since we don't have other choices:
    - Combination Problem: Finding a set of k numbers from N numbers based on certain rules.
    - Cutting Problem: Determining the number of ways to cut a string according to specific rules.
    - Subset Problem: Finding how many subsets of a set of N numbers meet certain conditions.
    - Permutation Problem: Finding the number of arrangements of N numbers according to specific rules.
    - Chessboard Problem: Problems like the N-Queens problem or solving Sudoku.

## LC 77 combinations
[Link](https://leetcode.com/problems/combinations/description/)   
[Cousrse Link](https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html)    

- Backtracking / recursion here is like flexible for loop that only stops when meet the return condition

- **parameter here works same as global variable for recursion for updating state**
```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        result = []  # 存放结果集
        self.backtracking(n, k, 1, [], result)
        return result
    def backtracking(self, n, k, startIndex, path, result):
        if len(path) == k:
            result.append(path[:])
            return
        for i in range(startIndex, n + 1):  # can prune: for i in range(startIndex, n - (k - len(path)) + 2):
            path.append(i)  # 处理节点
            self.backtracking(n, k, i + 1, path, result)
            path.pop()  # 回溯，撤销处理的节点
```
Time: **O(n * 2^n)** more like creating subset from n elemets
Space: **O(n)** 

##  LC 216 combination-sum-iii
[Link](https://leetcode.com/problems/combination-sum-iii/description/)   
[Cousrse Link](https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html)
  
- preety much like 77 but checking the sum
- remember to backtracking!
```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        result = []  # 存放结果集
        self.backtracking(n, k, 0, 1, [], result)
        return result

    def backtracking(self, targetSum, k, currentSum, startIndex, path, result):
        if currentSum > targetSum:  # 剪枝操作
            return  # 如果path的长度等于k但currentSum不等于targetSum，则直接返回
        if len(path) == k:
            if currentSum == targetSum:
                result.append(path[:])
            return
        for i in range(startIndex, 9 - (k - len(path)) + 2):  # 剪枝
            currentSum += i  # 处理
            path.append(i)  # 处理
            self.backtracking(targetSum, k, currentSum, i + 1, path, result)  # 注意i+1调整startIndex
            currentSum -= i  # 回溯
            path.pop()  # 回溯
```

Time: **O(n * 2^n)** more like creating subset from n elemets
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
