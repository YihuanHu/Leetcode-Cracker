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


##  LC 538 convert-bst-to-greater-tree
[Link](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)   
[Cousrse Link](https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html)
  
- recursive: Reverse inorder RVL
```python
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.pre = 0  # 记录前一个节点的数值
        self.traversal(root)
        return root
    def traversal(self, cur):
        if cur is None:
            return        
        self.traversal(cur.right)
        cur.val += self.pre
        self.pre = cur.val
        self.traversal(cur.left)
```


- typical iterative w/ queue 
```python
class Solution:
    def __init__(self):
        self.pre = 0  # 记录前一个节点的数值
    
    def traversal(self, root):
        stack = []
        cur = root
        while cur or stack:
            if cur:
                stack.append(cur)
                cur = cur.right  # 右
            else:
                cur = stack.pop()  # 中
                cur.val += self.pre
                self.pre = cur.val
                cur = cur.left  # 左
    
    def convertBST(self, root):
        self.pre = 0
        self.traversal(root)
        return root
```


##  Adds on
- Summary:
    - 涉及到二叉树的构造，无论普通二叉树还是二叉搜索树一定前序，都是先构造中节点。
    - 求普通二叉树的属性，一般是后序，一般要通过递归函数的返回值做计算。
    - 求二叉搜索树的属性，一定是中序了，要不白瞎了有序性了。
- [ ] binary tree summary [Link](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E6%80%BB%E7%BB%93%E7%AF%87.html#%E6%9C%80%E5%90%8E%E6%80%BB%E7%BB%93)
- [ ] viz summary [Link](https://github.com/YihuanHu/Leetcode-Cracker/blob/main/image/binary_tree_viz.png)
