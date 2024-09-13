
# Day15 Binary Tree Part3
# recursive 3 steps
1. Determine the parameters and return value of the recursive function.
2. Define the base (termination) condition.
3.Establish the logic for a single layer of recursion.

## LC 226 invert-binary-tree 
[Link](https://leetcode.com/problems/invert-binary-tree/description/)
[Cousrse Link](https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
-  Preorder
```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root


class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None      
        stack = [root]        
        while stack:
            node = stack.pop()   
            node.left, node.right = node.right, node.left                   
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)  
        return root
```

-  Inorder
```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        self.invertTree(root.left)
        root.left, root.right = root.right, root.left
        self.invertTree(root.left) // since it already being inverted
        return root


class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None      
        stack = [root]        
        while stack:
            node = stack.pop()                   
            if node.left:
                stack.append(node.left)
            node.left, node.right = node.right, node.left               
            if node.left:
                stack.append(node.left)       
        return root
```

- Postorder
```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        self.invertTree(root.left)
        self.invertTree(root.right)
        root.left, root.right = root.right, root.left
        return root

class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None      
        stack = [root]        
        while stack:
            node = stack.pop()                   
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)  
            node.left, node.right = node.right, node.left               
     
        return root
```


##  LC 101 symmetric-tree
[Link](https://leetcode.com/problems/symmetric-tree/description/)   
[Cousrse Link](https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF)

-  Postorder
```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        return self.compare(root.left, root.right)
        
    def compare(self, left, right):
        #首先排除空节点的情况
        if left == None and right != None: return False
        elif left != None and right == None: return False
        elif left == None and right == None: return True
        #排除了空节点，再排除数值不相同的情况
        elif left.val != right.val: return False
        
        #此时就是：左右节点都不为空，且数值相同的情况
        #此时才做递归，做下一层的判断
        outside = self.compare(left.left, right.right) #左子树：左、 右子树：右
        inside = self.compare(left.right, right.left) #左子树：右、 右子树：左
        isSame = outside and inside #左子树：中、 右子树：中 （逻辑处理）
        return isSame
```
-  Level order: the level value is symmetric
```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        
        queue = collections.deque([root.left, root.right])
        
        while queue:
            level_size = len(queue)
            
            if level_size % 2 != 0:
                return False
            
            level_vals = []
            for i in range(level_size):
                node = queue.popleft()
                if node:
                    level_vals.append(node.val)
                    queue.append(node.left)
                    queue.append(node.right)
                else:
                    level_vals.append(None)
                    
            if level_vals != level_vals[::-1]:
                return False
            
        return True
```
- Iterative with Stack
```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        st = [] #这里改成了栈
        st.append(root.left)
        st.append(root.right)
        while st:
            rightNode = st.pop()
            leftNode = st.pop()
            if not leftNode and not rightNode:
                continue
            if not leftNode or not rightNode or leftNode.val != rightNode.val:
                return False
            st.append(leftNode.left)
            st.append(rightNode.right)
            st.append(leftNode.right)
            st.append(rightNode.left)
        return True
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
