
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



## LC 104 maximum-depth-of-binary-tree
[Link](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)
[Cousrse Link](https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html#_102-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86)
- **Preorder traversal can be used to calculate the depth of a binary tree node.**
    - Depth refers to how far a node is from the root.
    - As you traverse from the root to any node, you're essentially following the path from the root downward, keeping track of the levels as you go
- **Postorder traversal can be used to calculate the height of a binary tree node.**
    - Height refers to how far a node is from the leaf nodes
    - By the time the traversal reaches the parent node, it has already calculated the height of its children, allowing the parent node to determine its own height as "1 + the maximum height of its children."
- The height of leaf node is the max depth

-  Iterative:
```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        depth = 0
        queue = collections.deque([root])
        
        while queue:
            depth += 1
            for _ in range(len(queue)):
                node = queue.popleft()
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return depth
```
-  Recursive:
```python
class Solution:
    def maxdepth(self, root: treenode) -> int:
        return self.getdepth(root)
        
    def getdepth(self, node):
        if not node:
            return 0
        leftheight = self.getdepth(node.left) #左
        rightheight = self.getdepth(node.right) #右
        height = 1 + max(leftheight, rightheight) #中
        return height

```

## LC 111 minimum-depth-of-binary-tree
[Link](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)
[Cousrse Link](https://programmercarl.com/0111.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- ??? What is diff of using post order and pre order?
  
-  Post order
```python
class Solution:
    def getDepth(self, node):
        if node is None:
            return 0
        leftDepth = self.getDepth(node.left)  # 左
        rightDepth = self.getDepth(node.right)  # 右
        
        # 当一个左子树为空，右不为空，这时并不是最低点
        if node.left is None and node.right is not None:
            return 1 + rightDepth
        
        # 当一个右子树为空，左不为空，这时并不是最低点
        if node.left is not None and node.right is None:
            return 1 + leftDepth
        
        result = 1 + min(leftDepth, rightDepth)
        return result

    def minDepth(self, root):
        return self.getDepth(root)
```
-  Level order: once it finds a leaf node then return
```python
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        depth = 0
        queue = collections.deque([root])
        
        while queue:
            depth += 1 
            for _ in range(len(queue)):
                node = queue.popleft()
                
                if not node.left and not node.right:
                    return depth
            
                if node.left:
                    queue.append(node.left)
                    
                if node.right:
                    queue.append(node.right)

        return depth
```
## Adds on
- [ ] LC 100 & 572 for symmetry tree
- [ ] LC 559 for findig depth
