# Day39 Dynamic Programming Part8

##  LC 121 best-time-to-buy-and-sell-stock
[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)   
[Cousrse Link](https://programmercarl.com/0121.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA.html)
- Greedy Algo: find the smallest on the left and largest on the right
- Steps for DP:
    - Define the dp[j]:
        - dp[i][0] represents the maximum cash obtained on day i when holding a stock
        - dp[i][1] represents the maximum cash obtained on day i when not holding a stock.
    - Define the state transition:
    - If hold a stock at day i , then we can either hold beofre day i or buy at day i: dp[i][0] = max(dp[i - 1][0], -prices[i])
    - If did not hold a stock at day i, then we can either sell it before day i or sell at day i: dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0])
    - How to initialize the DP array:
      - dp[0][0] -= prices[0]
      - dp[0][1] = 0
    - Determine the order of traversal: forward since [i-1] 
    - Provide an example to derive the DP array:
        - prices = [7,1,5,3,6,4]
        - dp = [[-7,0], [-1,0], [-1,4], [-1,4], [-1,5], [-1,5],]
```python
# solution 1:greedy algo
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        low = float("inf")
        result = 0
        for i in range(len(prices)):
            low = min(low, prices[i]) #取最左最小价格
            result = max(result, prices[i] - low) #直接取最大区间利润
        return result

    # solution 2: dp table
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        length = len(prices)
        if length == 0:
            return 0
        dp = [[0] * 2 for _ in range(length)]
        dp[0][0] = -prices[0]
        dp[0][1] = 0
        for i in range(1, length):
            dp[i][0] = max(dp[i-1][0], -prices[i])
            dp[i][1] = max(dp[i-1][1], prices[i] + dp[i-1][0])
        return dp[-1][1]
```
Time: **O(n)**                   
Space: **O(1)** for solution 1 and **O(n)** for solution 2

##  LC 122 best-time-to-buy-and-sell-stock-ii
[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)   
[Cousrse Link](https://programmercarl.com/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html)
- Like the above LC 121 but the change in state transition 
- Define the state transition:
    - If hold a stock at day i , then we can either hold beofre day i or buy at day i: dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i])
    - If did not hold a stock at day i, then we can either sell it before day i or sell at day i: dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0])

```python
# solution 1:DP table
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        length = len(prices)
        dp = [[0] * 2 for _ in range(length)]
        dp[0][0] = -prices[0]
        dp[0][1] = 0
        for i in range(1, length):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i]) #注意这里是和121. 买卖股票的最佳时机唯一不同的地方
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] + prices[i])
        return dp[-1][1]

# solution 2: 1D array
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        length = len(prices)
        dp = [[0] * 2 for _ in range(2)] #注意这里只开辟了一个2 * 2大小的二维数组
        dp[0][0] = -prices[0]
        dp[0][1] = 0
        for i in range(1, length):
            dp[i % 2][0] = max(dp[(i-1) % 2][0], dp[(i-1) % 2][1] - prices[i])
            dp[i % 2][1] = max(dp[(i-1) % 2][1], dp[(i-1) % 2][0] + prices[i])
        return dp[(length-1) % 2][1]

```
Time: **O(n)**                   
Space: **O(n)** for solution 1 and **O(1)** for solution 2

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

