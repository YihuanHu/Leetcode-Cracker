
# Day15 Binary Tree Part3
- 因为求深度可以从上到下去查 所以需要前序遍历（中左右），而高度只能从下到上去查，所以只能后序遍历（左右中）
- back tracking: recursion + prune
    - Backtracking and recursion should correspond one-to-one
    - e.g ` self.traversal(cur.left, path, result) #recursion`
            `path.pop()  # backtrack`

- when to use paremeter VS return values
    - Use Parameters When:
        - **Shared State**: You need to track or modify a shared state (like a path or solution) across recursive calls, especially in backtracking.
        - **In-Place Modification**: You can modify an object in place (e.g., appending/removing elements from a list) to avoid creating new copies at each recursion step.
        - **Efficiency**: Passing mutable objects (like lists) avoids the overhead of returning and copying new instances.
    - Use Return Values When:
        - **Combining Results**: Each recursive call returns a value that needs to be combined with others (e.g., summing, boolean checks, merging results from subtrees).
        - **Immutable Data**: You're working with immutable structures or prefer not to mutate the input data.
        - **Pure Computation**: The recursion is focused on building up results rather than maintaining intermediate states (e.g., calculating Fibonacci numbers or collecting results).

## LC 110 balanced-binary-tree
[Link](https://leetcode.com/problems/balanced-binary-tree/description/)
[Cousrse Link](https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E9%A2%98%E5%A4%96%E8%AF%9D)
-  Postorder: we need to find the diff of height
```python
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        if self.get_height(root) != -1:
            return True
        else:
            return False

    def get_height(self, root: TreeNode) -> int:
        # Base Case
        if not root:
            return 0
        # 左
        if (left_height := self.get_height(root.left)) == -1:
            return -1
        # 右
        if (right_height := self.get_height(root.right)) == -1:
            return -1
        # 中
        if abs(left_height - right_height) > 1:
            return -1
        else:
            return 1 + max(left_height, right_height)
```


##  LC 257 binary-tree-paths
[Link](https://leetcode.com/problems/binary-tree-paths/description/)   
[Cousrse Link](https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

- backtracking 

-  Preorder
```python
class Solution:
    def traversal(self, cur, path, result):
        path.append(cur.val)  # 中
        if not cur.left and not cur.right:  # 到达叶子节点
            sPath = '->'.join(map(str, path))
            result.append(sPath)
            return
        if cur.left:  # 左
            self.traversal(cur.left, path, result)
            path.pop()  # 回溯
        if cur.right:  # 右
            self.traversal(cur.right, path, result)
            path.pop()  # 回溯

    def binaryTreePaths(self, root):
        result = []
        path = []
        if not root:
            return result
        self.traversal(root, path, result)
        return result
```



## LC 404 sum-of-left-leaves
[Link](https://leetcode.com/problems/sum-of-left-leaves/description/)           
[Cousrse Link](https://programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html#%E6%80%9D%E8%B7%AF)
- Standing at the parent node to judge if the left child node is leaf

-  Postorder
```python
class Solution:
    def sumOfLeftLeaves(self, root):
        if root is None:
            return 0
        if root.left is None and root.right is None: # skip leaf node since cannot decide if this is the left
            return 0
        
        leftValue = self.sumOfLeftLeaves(root.left)  # 左
        if root.left and not root.left.left and not root.left.right:  # 左子树是左叶子的情况
            leftValue = root.left.val
            
        rightValue = self.sumOfLeftLeaves(root.right)  # 右

        sum_val = leftValue + rightValue  # 中
        return sum_val
```


## LC 222 count-complete-tree-nodes
[Link](https://leetcode.com/problems/count-complete-tree-nodes/description/)
[Cousrse Link](https://programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html)

-  Postorder:
```python
class Solution:
    def countNodes(self, root: TreeNode) -> int:
        return self.getNodesNum(root)
        
    def getNodesNum(self, cur):
        if not cur:
            return 0
        leftNum = self.getNodesNum(cur.left) #左
        rightNum = self.getNodesNum(cur.right) #右
        treeNum = leftNum + rightNum + 1 #中
        return treeNum
```


-  Based on the characteristics of complete tree:
    - subtree of the complete tree is a full tree
    - the total number of nodes of a full tree is 2^^depth -1
```python
class Solution:
    def countNodes(self, root: TreeNode) -> int:
        if not root:
            return 0
        left = root.left
        right = root.right
        leftDepth = 0 #这里初始为0是有目的的，为了下面求指数方便
        rightDepth = 0
        while left: #求左子树深度
            left = left.left
            leftDepth += 1
        while right: #求右子树深度
            right = right.right
            rightDepth += 1
        if leftDepth == rightDepth:
            return (2 << leftDepth) - 1 #注意(2<<1) 相当于2^2，所以leftDepth初始为0
        return self.countNodes(root.left) + self.countNodes(root.right) + 1
```
-  iterative queue
```python
import collections
class Solution:
    def countNodes(self, root: TreeNode) -> int:
        queue = collections.deque()
        if root:
            queue.append(root)
        result = 0
        while queue:
            size = len(queue)
            for i in range(size):
                node = queue.popleft()
                result += 1 #记录节点数量
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        return result
```
