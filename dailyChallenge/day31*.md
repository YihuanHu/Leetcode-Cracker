# Day31 Greedy Algo Part5

## LC 56 merge-intervals
[Link](https://leetcode.com/problems/merge-intervals/description/)   
[Cousrse Link](https://programmercarl.com/0056.%E5%90%88%E5%B9%B6%E5%8C%BA%E9%97%B4.html)    
- Using the overlapping format 
```python
class Solution:
    def merge(self, intervals):
        result = []
        if len(intervals) == 0:
            return result  # 区间集合为空直接返回

        intervals.sort(key=lambda x: x[0])  # 按照区间的左边界进行排序

        result.append(intervals[0])  # 第一个区间可以直接放入结果集中

        for i in range(1, len(intervals)):
            if result[-1][1] >= intervals[i][0]:  # 发现重叠区间
                # 合并区间，只需要更新结果集最后一个区间的右边界，因为根据排序，左边界已经是最小的
                result[-1][1] = max(result[-1][1], intervals[i][1])
            else:
                result.append(intervals[i])  # 区间不重叠

        return result
```
Time: **O(n*Logn)**     
Space: **O(n)** 

##  LC 738 monotone-increasing-digits
[Link](https://leetcode.com/problems/monotone-increasing-digits/description/)   
[Cousrse Link](https://programmercarl.com/0738.%E5%8D%95%E8%B0%83%E9%80%92%E5%A2%9E%E7%9A%84%E6%95%B0%E5%AD%97.html)
- e.g 32 -> 329 -> 299
- iterate backward
    - One counter example is: Number: 332. If we traverse from front to back, it becomes 329. At this point, 2 is still less than the first digit 3, so the correct result should be 299.    
- Local optimal: Once the condition strNum[i - 1] > strNum[i] occurs (non-monotonically), the first step is to decrease strNum[i - 1] by one and set strNum[i] to 9.
- Global optimal: get max the monotone increasing numbers
```python
class Solution:
    def monotoneIncreasingDigits(self, N: int) -> int:
        # 将整数转换为字符串
        strNum = list(str(N))

        # 从右往左遍历字符串
        for i in range(len(strNum) - 1, 0, -1):
            # 如果当前字符比前一个字符小，说明需要修改前一个字符
            if strNum[i - 1] > strNum[i]:
                strNum[i - 1] = str(int(strNum[i - 1]) - 1)  # 将前一个字符减1
                # 将修改位置后面的字符都设置为9，因为修改前一个字符可能破坏了递增性质
                strNum[i:] = '9' * (len(strNum) - i)

        # 将列表转换为字符串，并将字符串转换为整数并返回
        return int(''.join(strNum))

```
Time: **O(n)**     
Space: **O(n)** 


## * LC 968 binary-tree-cameras
[Link](https://leetcode.com/problems/binary-tree-cameras/description/)   
[Cousrse Link](https://programmercarl.com/0763.%E5%88%92%E5%88%86%E5%AD%97%E6%AF%8D%E5%8C%BA%E9%97%B4.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)      
- Use postorder so that we can start iterate from bottom to top
- 3 states:
    - 0: The node is uncovered
    - 1: The node has a camera
    - 2: The node is covered
- 4 cases
- What about null node?
    - cannot be in an uncovered state, as this would require placing a camera on the leaf node
    - cannot have a camera, as this would make it unnecessary to place a camera on the parent node of the leaf node, instead, the camera could be placed on the grandparent node of the leaf
    - Therefore, the state of an empty node can only be covered, allowing us to place a camera on the parent node of the leaf node
- Local optimality: Place cameras on the parent nodes of the leaf nodes to minimize the number of cameras used
- Global optimality: Minimize the total number of cameras used overal
```python
class Solution:
         # Greedy Algo:
        # 从下往上安装摄像头：跳过leaves这样安装数量最少，局部最优 -> 全局最优
        # 先给leaves的父节点安装，然后每隔两层节点安装一个摄像头，直到Head
        # 0: 该节点未覆盖
        # 1: 该节点有摄像头
        # 2: 该节点有覆盖
    def minCameraCover(self, root: TreeNode) -> int:
        # 定义递归函数
        result = [0]  # 用于记录摄像头的安装数量
        # 情况4：头结点没有覆盖
        if self.traversal(root, result) == 0:
            result[0] += 1

        return result[0]

        
    def traversal(self, cur: TreeNode, result: List[int]) -> int:
        if not cur:
            return 2

        left = self.traversal(cur.left, result)
        right = self.traversal(cur.right, result)

        # 情况1: 左右节点都有覆盖, 中间节点应该就是无覆盖的状态
        if left == 2 and right == 2:
            return 0

        # 情况2: 左右节点至少有一个无覆盖的情况,中间节点（父节点）应该放摄像头
        # left == 0 && right == 0 左右节点无覆盖
        # left == 1 && right == 0 左节点有摄像头，右节点无覆盖
        # left == 0 && right == 1 左节点无覆盖，右节点有摄像头
        # left == 0 && right == 2 左节点无覆盖，右节点覆盖
        # left == 2 && right == 0 左节点覆盖，右节点无覆盖
        if left == 0 or right == 0:
            result[0] += 1
            return 1

        # 情况3: 左右节点至少有一个有摄像头,其父节点就应该是2（覆盖的状态）
        # left == 1 && right == 2 左节点有摄像头，右节点有覆盖
        # left == 2 && right == 1 左节点有覆盖，右节点有摄像头
        # left == 1 && right == 1 左右节点都有摄像头
        if left == 1 or right == 1:
            return 2


```
Time: **O(n)**     
Space: **O(n)** 

## Adds on
- [ ] greedy algo summary [Link](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93%E7%AF%87.html#%E8%B4%AA%E5%BF%83%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80)
    - No specific cheatsheet to follow
    - Find out local/global locality 
