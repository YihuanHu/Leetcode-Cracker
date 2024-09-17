
# Day17 Binary Tree Part5


## LC 654 maximum-binary-tree

[Link](https://leetcode.com/problems/maximum-binary-tree/description/)   
[Cousrse Link](https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html)    

-  Preorder: usually used to construct a binary tree
```python
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> TreeNode:
        if len(nums) == 1:
            return TreeNode(nums[0])
        node = TreeNode(0)
        # 找到数组中最大的值和对应的下标
        maxValue = 0
        maxValueIndex = 0
        for i in range(len(nums)):
            if nums[i] > maxValue:
                maxValue = nums[i]
                maxValueIndex = i
        node.val = maxValue
        # 最大值所在的下标左区间 构造左子树
        if maxValueIndex > 0:
            new_list = nums[:maxValueIndex]
            node.left = self.constructMaximumBinaryTree(new_list)
        # 最大值所在的下标右区间 构造右子树
        if maxValueIndex < len(nums) - 1:
            new_list = nums[maxValueIndex+1:]
            node.right = self.constructMaximumBinaryTree(new_list)
        return node
```

##  LC 617 merge-two-binary-trees
[Link](https://leetcode.com/problems/merge-two-binary-trees/description/)   
[Cousrse Link](https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html)

- 
-  preorder (all the orders are fine)
```python
class Solution:
    def mergeTrees(self, root1: TreeNode, root2: TreeNode) -> TreeNode:
        # 递归终止条件: 
        #  但凡有一个节点为空, 就立刻返回另外一个. 如果另外一个也为None就直接返回None. 
        if not root1: 
            return root2
        if not root2: 
            return root1
        # 上面的递归终止条件保证了代码执行到这里root1, root2都非空. 
        root1.val += root2.val # 中
        root1.left = self.mergeTrees(root1.left, root2.left) #左
        root1.right = self.mergeTrees(root1.right, root2.right) # 右
        
        return root1 # ⚠️ 注意: 本题我们重复使用了题目给出的节点而不是创建新节点. 节省时间, 空间. 
 
```



## *LC 700 search-in-a-binary-search-tree
[Link](https://leetcode.com/problems/search-in-a-binary-search-tree/description/)           
[Cousrse Link](https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- **Binary Search Tree:**
    -  If the left subtree is not empty, all node values in the left subtree are less than the root's value
    -  If the right subtree is not empty, all node values in the right subtree are greater than the root's value
    -  Both the left and right subtrees are also binary search trees

-  recursive
```python
class Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        # 为什么要有返回值: 
        #   因为搜索到目标节点就要立即return，
        #   这样才是找到节点就返回（搜索某一条边），如果不加return，就是遍历整棵树了。

        if not root or root.val == val: 
            return root

        if root.val > val: 
            return self.searchBST(root.left, val)

        if root.val < val: 
            return self.searchBST(root.right, val)
```

- iterative/BFS: NO need to backtracking since BTS already has a order that we can follow along the way
```python
class Solution:
    def searchBST(self, root: TreeNode, val: int) -> TreeNode:
        while root:
            if val < root.val: root = root.left
            elif val > root.val: root = root.right
            else: return root
        return None
```

## LC 98 validate-binary-search-tree
[Link](https://leetcode.com/problems/validate-binary-search-tree/description/)           
[Cousrse Link](https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)
- **Binary Search Tree:**
    -  If the left subtree is not empty, all node values in the left subtree are less than the root's value
    -  If the right subtree is not empty, all node values in the right subtree are greater than the root's value
    -  Both the left and right subtrees are also binary search trees

-  **Inorder is special for BST**: convert BST to vector and check if this is mononic increasing
```python
class Solution:
    def __init__(self):
        self.vec = []

    def traversal(self, root):
        if root is None:
            return
        self.traversal(root.left)
        self.vec.append(root.val)  # 将二叉搜索树转换为有序数组
        self.traversal(root.right)

    def isValidBST(self, root):
        self.vec = []  # 清空数组
        self.traversal(root)
        for i in range(1, len(self.vec)):
            # 注意要小于等于，搜索树里不能有相同元素
            if self.vec[i] <= self.vec[i - 1]:
                return False
        return True
```

- Simplified version: use pre
```python
class Solution:
    def __init__(self):
        self.pre = None  # 用来记录前一个节点

    def isValidBST(self, root):
        if root is None:
            return True

        left = self.isValidBST(root.left)

        if self.pre is not None and self.pre.val >= root.val:
            return False
        self.pre = root  # 记录前一个节点

        right = self.isValidBST(root.right)
        return left and right
```
