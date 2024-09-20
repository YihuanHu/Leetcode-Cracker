# Day21 Binary Tree Part8


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

##  LC 701 insert-into-a-binary-search-tree
[Link](https://leetcode.com/problems/insert-into-a-binary-search-tree/description/)   
[Cousrse Link](https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html#%E6%80%9D%E8%B7%AF)

- **It's a shame to traverse the whole BST with a target**
  
- recursive: no need for root so order doesn't matter
```python
class Solution:
    def insertIntoBST(self, root, val):
        if root is None:
            node = TreeNode(val)
            return node

        if root.val > val:
            root.left = self.insertIntoBST(root.left, val) # assign the parent-child relationship for the newly added node
        if root.val < val:
            root.right = self.insertIntoBST(root.right, val) # same

        return root
```

- level order + two pointers for assign a new node
```python
class Solution:
    def insertIntoBST(self, root, val):
        if root is None:  # 如果根节点为空，创建新节点作为根节点并返回
            node = TreeNode(val)
            return node

        cur = root
        parent = root  # 记录上一个节点，用于连接新节点
        while cur is not None:
            parent = cur
            if cur.val > val:
                cur = cur.left
            else:
                cur = cur.right

        node = TreeNode(val)
        if val < parent.val:
            parent.left = node  # 将新节点连接到父节点的左子树
        else:
            parent.right = node  # 将新节点连接到父节点的右子树

        return root
```

##  LC 450 delete-node-in-a-bst
[Link](https://leetcode.com/problems/delete-node-in-a-bst/description/)   
[Cousrse Link](https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)

- 5 cases to consider:
- 
  
- recursive: no need for root so order doesn't matter
```python
class Solution:
    def insertIntoBST(self, root, val):
        if root is None:
            node = TreeNode(val)
            return node

        if root.val > val:
            root.left = self.insertIntoBST(root.left, val) # assign the parent-child relationship for the newly added node
        if root.val < val:
            root.right = self.insertIntoBST(root.right, val) # same

        return root
```
