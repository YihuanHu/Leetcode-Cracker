# Day18 Binary Tree Part6


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
    def getMinimumDifference(self, root):
        stack = []
        cur = root
        pre = None
        result = float('inf')

        while cur is not None or len(stack) > 0:
            if cur is not None:
                stack.append(cur)  # 将访问的节点放进栈
                cur = cur.left  # 左
            else:
                cur = stack.pop()
                if pre is not None:  # 中
                    result = min(result, cur.val - pre.val)
                pre = cur
                cur = cur.right  # 右

        return result
```

##  LC 501 find-mode-in-binary-search-tree
[Link](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)   
[Cousrse Link](https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html#%E6%80%9D%E8%B7%AF)


- inorder
- two pointers
- why we use set: consider there might be several modes 
```python
class Solution:
    def __init__(self):
        self.maxCount = 0  # 最大频率
        self.count = 0  # 统计频率
        self.pre = None
        self.result = []

    def searchBST(self, cur):
        if cur is None:
            return

        self.searchBST(cur.left)  # 左
        # 中
        if self.pre is None:  # 第一个节点
            self.count = 1
        elif self.pre.val == cur.val:  # 与前一个节点数值相同
            self.count += 1
        else:  # 与前一个节点数值不同
            self.count = 1
        self.pre = cur  # 更新上一个节点

        if self.count == self.maxCount:  # 如果与最大值频率相同，放进result中
            self.result.append(cur.val)

        if self.count > self.maxCount:  # 如果计数大于最大值频率
            self.maxCount = self.count  # 更新最大频率
            self.result = [cur.val]  # 很关键的一步，不要忘记清空result，之前result里的元素都失效了

        self.searchBST(cur.right)  # 右
        return

    def findMode(self, root):
        self.count = 0
        self.maxCount = 0
        self.pre = None  # 记录前一个节点
        self.result = []

        self.searchBST(root)
        return self.result
```

- queue / level order
- same logic 
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
