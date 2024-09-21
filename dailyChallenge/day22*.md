# Day22 Backtracking Part1

## Backtracking 
- **Backtracking is a byproduct of recursion**; whenever there is recursion, backtracking will be present
- Backtracking fundamentally involves **exhaustive search**, exploring all possibilities to find the desired solution. While pruning techniques can enhance efficiency
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
    = Permutation Problem: Finding the number of arrangements of N numbers according to specific rules.
    - Chessboard Problem: Problems like the N-Queens problem or solving Sudoku.

## LC 669 trim-a-binary-search-tree

[Link](https://leetcode.com/problems/trim-a-binary-search-tree/submissions/1396124674/)   
[Cousrse Link](https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)    

- If the node is within the range, return the node
- If the node is out of range, return its right/left


- recursive
```python
class Solution:
    def trimBST(self, root: TreeNode, low: int, high: int) -> TreeNode:
        if root is None:
            return None
        if root.val < low:
            # 寻找符合区间 [low, high] 的节点
            return self.trimBST(root.right, low, high)
        if root.val > high:
            # 寻找符合区间 [low, high] 的节点
            return self.trimBST(root.left, low, high)
        root.left = self.trimBST(root.left, low, high)  # root.left 接入符合条件的左孩子
        root.right = self.trimBST(root.right, low, high)  # root.right 接入符合条件的右孩子
        return root
```

##  LC 108 convert-sorted-array-to-binary-search-tree
[Link](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)   
[Cousrse Link](https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E6%80%9D%E8%B7%AF)
  
- recursive + [] + avoid to pass array and use left right pointers
```python
class Solution:
    def traversal(self, nums: List[int], left: int, right: int) -> TreeNode:
        if left > right:
            return None
        
        mid = left + (right - left) // 2
        root = TreeNode(nums[mid])
        root.left = self.traversal(nums, left, mid - 1)
        root.right = self.traversal(nums, mid + 1, right)
        return root
    
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        root = self.traversal(nums, 0, len(nums) - 1)
        return root
```

- level order w/ 3 queues (iteration / Left / Right)
```python
from collections import deque

class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        if len(nums) == 0:
            return None
        
        root = TreeNode(0)  # 初始根节点
        nodeQue = deque()   # 放遍历的节点
        leftQue = deque()   # 保存左区间下标
        rightQue = deque()  # 保存右区间下标
        
        nodeQue.append(root)               # 根节点入队列
        leftQue.append(0)                  # 0为左区间下标初始位置
        rightQue.append(len(nums) - 1)     # len(nums) - 1为右区间下标初始位置

        while nodeQue:
            curNode = nodeQue.popleft()
            left = leftQue.popleft()
            right = rightQue.popleft()
            mid = left + (right - left) // 2

            curNode.val = nums[mid]  # 将mid对应的元素给中间节点

            if left <= mid - 1:  # 处理左区间
                curNode.left = TreeNode(0)
                nodeQue.append(curNode.left)
                leftQue.append(left)
                rightQue.append(mid - 1)

            if right >= mid + 1:  # 处理右区间
                curNode.right = TreeNode(0)
                nodeQue.append(curNode.right)
                leftQue.append(mid + 1)
                rightQue.append(right)

        return root
```

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
