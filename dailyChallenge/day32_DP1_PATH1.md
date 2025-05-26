# Day32 Dynamic Programming Part1
## DP
-  DP VS Greedy Algo
    - DP: each state is derived from the previous state
    - Greedy:  directly select the optimal choice from local information
- Steps for DP:
    - Define the dp[i] 
    - Define the state transition 
    - How to initialize the DP array
    - Determine the order of traversal
    - Provide an example to derive the DP array
- How to debug:
    - Did I derive the state transition formula for this problem with an example?
    - Did I print the logs of the DP array?
    - Does the printed DP array match what I expected?
      
## LC 509 fibonacci-number
[Link](https://leetcode.com/problems/fibonacci-number/description/)   
[Cousrse Link](https://programmercarl.com/0509.%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)    
- Steps for DP:
    - Define the dp[i]: The Fibonacci value of the i-th number is dp[i]
    - Define the state transition: problem states dp[i] = dp[i - 1] + dp[i - 2]
    - How to initialize the DP array: problem states dp[0] = 0; dp[1] = 1
    - Determine the order of traversal: we can see that dp[i] depends on dp[i - 1] and dp[i - 2] so must iterate front to back
    - Provide an example to derive the DP array: dp[10] = 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55
```python
# solution 1
class Solution: dp table
    def fib(self, n: int) -> int:
       
        # 排除 Corner Case
        if n == 0:
            return 0
        
        # 创建 dp table 
        dp = [0] * (n + 1)

        # 初始化 dp 数组
        dp[0] = 0
        dp[1] = 1

        # 遍历顺序: 由前向后。因为后面要用到前面的状态
        for i in range(2, n + 1):

            # 确定递归公式/状态转移公式
            dp[i] = dp[i - 1] + dp[i - 2]
        
        # 返回答案
        return dp[n]

# solution 2: use only 2 variables
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1:
            return n
        
        prev1, prev2 = 0, 1
        
        for _ in range(2, n + 1):
            curr = prev1 + prev2
            prev1, prev2 = prev2, curr
        
        return prev2
```
Time: **O(n)**     
Space: **O(n)** for solution 1 and **O(1)** for solution 2


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

