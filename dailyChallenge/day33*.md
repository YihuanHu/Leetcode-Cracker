# Day33 Dynamic Programming Part2

## LC 62 unique-paths
[Link](https://leetcode.com/problems/unique-paths/)   
[Cousrse Link](https://programmercarl.com/0062.%E4%B8%8D%E5%90%8C%E8%B7%AF%E5%BE%84.html)    
- Steps for DP:
    - Define the dp[i][j]: the number of different paths from (0, 0) to (i, j)
    - Define the state transition: dp[i][j] = dp[i - 1][j] + dp[i][j - 1] we only have 2 ways to reach dp[i][j]
    - How to initialize the DP array: since we can only go down or right, we only one solution for those edges
        - for i in range(m):dp[i][0] = 1
        - for j in range(n):dp[0][j] = 1
    - Determine the order of traversal: we can see that dp[i][j] depends on dp[i - 1] and dp[j - 1] so must iterate front to back
    - Provide an example to derive the DP array: try m,k = 3,3
```python
# solution 1: dp table
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # 创建一个二维列表用于存储唯一路径数
        dp = [[0] * n for _ in range(m)]
        
        # 设置第一行和第一列的基本情况
        for i in range(m):
            dp[i][0] = 1
        for j in range(n):
            dp[0][j] = 1
        
        # 计算每个单元格的唯一路径数
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        
        # 返回右下角单元格的唯一路径数
        return dp[m - 1][n - 1]


# solution 2: use roll
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # 创建一个一维列表用于存储每列的唯一路径数
        dp = [1] * n
        
        # 计算每个单元格的唯一路径数
        for j in range(1, m):
            for i in range(1, n):
                dp[i] += dp[i - 1]
        
        # 返回右下角单元格的唯一路径数
        return dp[n - 1]
```
Time: **O(m*n)**     
Space: **O(m*n)** for solution 1 and **O(n)** for solution 2


##  LC 70 climbing-stairs
[Link](https://leetcode.com/problems/climbing-stairs/description/)   
[Cousrse Link](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF.html)
- Steps for DP:
    - Define the dp[i]: To reach the i-th step of the staircase, there are dp[i] ways to do so
    - Define the state transition: dp[i] = dp[i - 1] + dp[i - 2]
        - there are only two ways reach dp[i],which are dp[i - 1] and dp[i-2]
        - If there are dp[i - 1] ways to reach the i-1-th step, then taking one more step to reach the i-th step gives us dp[i]
        - If there are dp[i - 2] ways to reach the i-2-th step, then taking two steps at once to reach the i-th step also gives us dp[i]
    - How to initialize the DP array: since n is postive integer so ignore 0 dp[1] = 1，dp[2] = 2
    - Determine the order of traversal: we can see that dp[i] depends on dp[i - 1] and dp[i - 2] so must iterate front to back
    - Provide an example to derive the DP array: dp[5] = 1, 1, 2, 3, 5
```python
# solution 1: dp table
# 空间复杂度为O(n)版本
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 1:
            return n
        
        dp = [0] * (n + 1)
        dp[1] = 1
        dp[2] = 2
        
        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        
        return dp[n]

# solution 2: use only 2 variables
# 空间复杂度为O(1)版本
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 1:
            return n
        
        prev1 = 1
        prev2 = 2
        
        for i in range(3, n + 1):
            total = prev1 + prev2
            prev1 = prev2
            prev2 = total
        
        return prev2

```
Time: **O(n)**     
Space: **O(n)** for solution 1 and **O(1)** for solution 2


##  LC 746 min-cost-climbing-stairs
[Link](https://leetcode.com/problems/min-cost-climbing-stairs/description/)   
[Cousrse Link](https://programmercarl.com/0746.%E4%BD%BF%E7%94%A8%E6%9C%80%E5%B0%8F%E8%8A%B1%E8%B4%B9%E7%88%AC%E6%A5%BC%E6%A2%AF.html)
- Steps for DP:
    - Define the dp[i]: To reach the i-th step of the staircase with the min cost
    - Define the state transition: dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])
        - there are only two ways reach dp[i],which are dp[i - 1] and dp[i-2]
        - If there are dp[i - 1] ways to reach the i-1-th step, then taking one more step to reach the i-th step gives us dp[i]
        - If there are dp[i - 2] ways to reach the i-2-th step, then taking two steps at once to reach the i-th step also gives us dp[i]
    - How to initialize the DP array: dp[0] = 0，dp[1] = 0 
    - Determine the order of traversal: we can see that dp[i] depends on dp[i - 1] and dp[i - 2] so must iterate front to back
    - Provide an example to derive the DP array: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1] min cost = 6
```python
# solution 1: dp table
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        dp = [0] * (len(cost) + 1)
        dp[0] = 0  # 初始值，表示从起点开始不需要花费体力
        dp[1] = 0  # 初始值，表示经过第一步不需要花费体力
        
        for i in range(2, len(cost) + 1):
            # 在第i步，可以选择从前一步（i-1）花费体力到达当前步，或者从前两步（i-2）花费体力到达当前步
            # 选择其中花费体力较小的路径，加上当前步的花费，更新dp数组
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])
        
        return dp[len(cost)]  # 返回到达楼顶的最小花费


# solution 2: use only 2 variables
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        dp0 = 0  # 初始值，表示从起点开始不需要花费体力
        dp1 = 0  # 初始值，表示经过第一步不需要花费体力
        
        for i in range(2, len(cost) + 1):
            # 在第i步，可以选择从前一步（i-1）花费体力到达当前步，或者从前两步（i-2）花费体力到达当前步
            # 选择其中花费体力较小的路径，加上当前步的花费，得到当前步的最小花费
            dpi = min(dp1 + cost[i - 1], dp0 + cost[i - 2])
            
            dp0 = dp1  # 更新dp0为前一步的值，即上一次循环中的dp1
            dp1 = dpi  # 更新dp1为当前步的最小花费
        
        return dp1  # 返回到达楼顶的最小花费
```
Time: **O(n)**     
Space: **O(n)** for solution 1 and **O(1)** for solution 2

