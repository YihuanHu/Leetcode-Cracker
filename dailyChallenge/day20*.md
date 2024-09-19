# Day20 Binary Tree Part7


## LC 235 lowest-common-ancestor-of-a-binary-search-tree

[Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)   
[Cousrse Link](https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html#%E6%80%9D%E8%B7%AF)    

- Similar to 236
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

##  * LC 236 lowest-common-ancestor-of-a-binary-tree
[Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)   
[Cousrse Link](https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html)

- **Postorder is kinda of backtracking**
- Diff in search in an edge VS the entire tree
1.  Searching an Edge:
```python
if recursive_function(root.left):
    return
if recursive_function(root.right):
    return
```
2. Search the entire tree

   
```python
left = recursive_function(root.left)   # Left
right = recursive_function(root.right) # Right
# Process logic involving left and right results (Center)
```   

- postorder : To find the lowest common ancestor, we need to traverse the tree from the bottom up, which can only be achieved using postorder traversal (i.e., backtracking)
- During backtracking, the entire tree must be traversed, even if the result is found, because the return values (e.g., left and right) are used for logical decisions
```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if root == q or root == p or root is None:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left is not None and right is not None:
            return root

        if left is None and right is not None:
            return right
        elif left is not None and right is None:
            return left
        else: 
            return None
```
