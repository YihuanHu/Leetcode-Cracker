# Day21 Binary Tree Part8


## LC 235 lowest-common-ancestor-of-a-binary-search-tree

[Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)   
[Cousrse Link](https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html#%E6%80%9D%E8%B7%AF)    

- Similar to 236
    - But 235 is searching on an edge since again it's BST
- Logic:
    -  Since this is BST, the ancestor must be [p,q]
    -  when we recursively traverse from top to bottom, the first time we encounter the cur node whose value is within the range [q, p], then cur is the lowest common ancestor of q and p
- why we need p&q as parameter here: for comparisons. Global variables also works but not flexible enough


- recursive (order does not matter)
```python
class Solution:
    def traversal(self, cur, p, q):
        if cur is None:
            return cur
                                                        # 中
        if cur.val > p.val and cur.val > q.val:           # 左
            left = self.traversal(cur.left, p, q)
            if left is not None:
                return left

        if cur.val < p.val and cur.val < q.val:           # 右
            right = self.traversal(cur.right, p, q)
            if right is not None:
                return right

        return cur

    def lowestCommonAncestor(self, root, p, q):
        return self.traversal(root, p, q)

## simplified version
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```


- iterative: simple for BST since it has an order 
```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        while root:
            if root.val > p.val and root.val > q.val:
                root = root.left
            elif root.val < p.val and root.val < q.val:
                root = root.right
            else:
                return root
        return None
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
