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

##  LC 108 convert-sorted-array-to-binary-search-tree
[Link](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)   
[Cousrse Link](https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E6%80%9D%E8%B7%AF)
  
- recursive + [] + avoid to pass array but use left right pointers
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
