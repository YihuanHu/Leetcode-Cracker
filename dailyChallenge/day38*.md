# Day38 Dynamic Programming Part7

##  LC 198 house-robber
[Link](https://leetcode.com/problems/house-robber/description/)   
[Cousrse Link](https://programmercarl.com/0198.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D.html)
- Not about combinations or permutations compared with LC 518 for combinations LC 377 for permutations
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:Considering the houses up to index i (including i), dp[i] represents the maximum amount that can be stolen
    - Define the state transition: dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]) whether or not to steal nth house
    - How to initialize the DP array: dp[0] = nums[0] dp[1] = max(nums[0], nums[1])
    - Determine the order of traversal: forward since [i-1] and [i-2]
    - Provide an example to derive the DP array:
        - nums = [2,7,9,3,1]
        - dp = [2,7,11,11,12]
```python
# solution 1:dp table
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:  # 如果没有房屋，返回0
            return 0

        n = len(nums)
        dp = [[0, 0] for _ in range(n)]  # 创建二维动态规划数组，dp[i][0]表示不抢劫第i个房屋的最大金额，dp[i][1]表示抢劫第i个房屋的最大金额

        dp[0][1] = nums[0]  # 抢劫第一个房屋的最大金额为第一个房屋的金额

        for i in range(1, n):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1])  # 不抢劫第i个房屋，最大金额为前一个房屋抢劫和不抢劫的最大值
            dp[i][1] = dp[i-1][0] + nums[i]  # 抢劫第i个房屋，最大金额为前一个房屋不抢劫的最大金额加上当前房屋的金额

        return max(dp[n-1][0], dp[n-1][1])  # 返回最后一个房屋中可抢劫的最大金额

    # solution 2: 1D array
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 0:  # 如果没有房屋，返回0
            return 0
        if len(nums) == 1:  # 如果只有一个房屋，返回其金额
            return nums[0]

        # 创建一个动态规划数组，用于存储最大金额
        dp = [0] * len(nums)
        dp[0] = nums[0]  # 将dp的第一个元素设置为第一个房屋的金额
        dp[1] = max(nums[0], nums[1])  # 将dp的第二个元素设置为第一二个房屋中的金额较大者

        # 遍历剩余的房屋
        for i in range(2, len(nums)):
            # 对于每个房屋，选择抢劫当前房屋和抢劫前一个房屋的最大金额
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1])

        return dp[-1]  # 返回最后一个房屋中可抢劫的最大金额
```
Time: **O(n)**                 
Space: **O(n)** 

##  LC 213 house-robber-ii
[Link](https://leetcode.com/problems/house-robber-ii/)   
[Cousrse Link](https://programmercarl.com/0213.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8DII.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- Like the above LC 198 but there is a loop
- So three cases to consider to never rob both the first and last houses at the same time. We only need 2nd and 3rd cases since first is being part of them.
    - Exclude start & end
    - Exclude start
    - Exclude end 

```python
# solution 1:DP table
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) < 3:
            return max(nums)

        # 情况二：不抢劫第一个房屋
        result1 = self.robRange(nums[:-1])

        # 情况三：不抢劫最后一个房屋
        result2 = self.robRange(nums[1:])

        return max(result1, result2)

    def robRange(self, nums):
        dp = [[0, 0] for _ in range(len(nums))]
        dp[0][1] = nums[0]

        for i in range(1, len(nums)):
            dp[i][0] = max(dp[i - 1])
            dp[i][1] = dp[i - 1][0] + nums[i]

        return max(dp[-1])

# solution 2: 1D array
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        if len(nums) == 1:
            return nums[0]
        
        result1 = self.robRange(nums, 0, len(nums) - 2)  # 情况二
        result2 = self.robRange(nums, 1, len(nums) - 1)  # 情况三
        return max(result1, result2)
    # 198.打家劫舍的逻辑
    def robRange(self, nums: List[int], start: int, end: int) -> int:
        if end == start:
            return nums[start]
        
        prev_max = nums[start]
        curr_max = max(nums[start], nums[start + 1])
        
        for i in range(start + 2, end + 1):
            temp = curr_max
            curr_max = max(prev_max + nums[i], curr_max)
            prev_max = temp
        
        return curr_max

```
Time: **O(n)**              
Space: **O(n)** 

##  LC 337 house-robber-iii
[Link](https://leetcode.com/problems/house-robber-iii/description/)   
[Cousrse Link](https://programmercarl.com/0337.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8DIII.html)
- Like the above LC 198 but there is a binary tree
- Steps for Binary Tree:
    - returning parameters and values:
        - return dp array of length 2 since recursion gonna save the parameters in each stack
        - Index 0 represents the maximum amount of money obtained if the current node is not robbed.
        - Index 1 represents the maximum amount of money obtained if the current node is robbed.
    - When to return: if node is null, retuen (0,0)
    - Order: We need to use postorder since both LR need to be considered for stealing before make a decision
    - What we do for each recursion:
        - conside stealing child's node: val_0 = max(left[0], left[1]) + max(right[0], right[1])
        - steal cur node: val_1 = node.val + left[0] + right[0]
        - **Remember when we steal child's node, we are not 100% sure to steal child's node so we just pick the max values out of it**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        # dp数组（dp table）以及下标的含义：
        # 1. 下标为 0 记录 **不偷该节点** 所得到的的最大金钱
        # 2. 下标为 1 记录 **偷该节点** 所得到的的最大金钱
        dp = self.traversal(root)
        return max(dp)

    # 要用后序遍历, 因为要通过递归函数的返回值来做下一步计算
    def traversal(self, node):
        
        # 递归终止条件，就是遇到了空节点，那肯定是不偷的
        if not node:
            return (0, 0)

        left = self.traversal(node.left)
        right = self.traversal(node.right)

        # 不偷当前节点, 偷子节点
        val_0 = max(left[0], left[1]) + max(right[0], right[1])

        # 偷当前节点, 不偷子节点
        val_1 = node.val + left[0] + right[0]

        return (val_0, val_1)
```
Time: **O(n)**              
Space: **O(log n)** 

