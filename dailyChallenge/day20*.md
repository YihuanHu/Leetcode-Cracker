# Day20 Binary Tree Part7


## LC 530 minimum-absolute-difference-in-bst

[Link](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)   
[Cousrse Link](https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html)    

-  inorder: which is aligned with BST increasing trend
-  two pointers
```python
class Solution:
    def __init__(self):
        self.result = float('inf')
        self.pre = None

    def traversal(self, cur):
        if cur is None:
            return
        self.traversal(cur.left)  # 左
        if self.pre is not None:  # 中
            self.result = min(self.result, cur.val - self.pre.val)
        self.pre = cur  # 记录前一个
        self.traversal(cur.right)  # 右

    def getMinimumDifference(self, root):
        self.traversal(root)
        return self.result
```

- iterative / queue
```python
class Solution:
    def findMode(self, root):
        st = []
        cur = root
        pre = None
        maxCount = 0  # 最大频率
        count = 0  # 统计频率
        result = []

        while cur is not None or st:
            if cur is not None:  # 指针来访问节点，访问到最底层
                st.append(cur)  # 将访问的节点放进栈
                cur = cur.left  # 左
            else:
                cur = st.pop()
                if pre is None:  # 第一个节点
                    count = 1
                elif pre.val == cur.val:  # 与前一个节点数值相同
                    count += 1
                else:  # 与前一个节点数值不同
                    count = 1

                if count == maxCount:  # 如果和最大值相同，放进result中
                    result.append(cur.val)

                if count > maxCount:  # 如果计数大于最大值频率
                    maxCount = count  # 更新最大频率
                    result = [cur.val]  # 很关键的一步，不要忘记清空result，之前result里的元素都失效了

                pre = cur
                cur = cur.right  # 右

        return result
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
