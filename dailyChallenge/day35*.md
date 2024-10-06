# Day35 Dynamic Programming Part4

## LC 1049 last-stone-weight-ii
[Link](https://leetcode.com/problems/last-stone-weight-ii/description/)   
[Cousrse Link](https://programmercarl.com/1049.%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8FII.html)    
- like a 0/1 bag problem where we trying to find out two divided subset which near sum / 2 e.g LC 416 partition-equal-subset-sum
- we get target = sum / 2 by floor function so sum - dp[target] > dp[target] and we return this diff as the remaining stone
- Steps for DP:
    - Define the dp[j]:
        - a knapsack with a capacity of j, the maximum weight that can be carried is represented by dp[j]
    - Define the state transition: **dp[j] = max(dp[j], dp[j - stones[i]] + stones[i])**
    - How to initialize the DP array: 
        -  we initiate whole dp as 0 since all are postive integer
        -  if we have negative, we should iniate it as negative infinite
    - Determine the order of traversal: **reverse loop for bag and item first then bag** see Day 34 LC 416 
    - Provide an example to derive the DP array:
        - stones = [2,4,1,1] => bag weight = (2+4+1+1)/2 = 4 
        - dp = [0,1,2,3,4]
```python
# rolling array/1 D array
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        dp = [0] * 15001
        total_sum = sum(stones)
        target = total_sum // 2

        for stone in stones:  # 遍历物品
            for j in range(target, stone - 1, -1):  # 遍历背包
                dp[j] = max(dp[j], dp[j - stone] + stone)

        return total_sum - dp[target] - dp[target]

```
Time: **O(m*n)**     
Space: **O(n)** 


##  LC 494 target-sum
[Link](https://leetcode.com/problems/target-sum/description/)   
[Cousrse Link](https://programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html)
- How to transfer this combination question into a 0/1 bag problem:
    - assume all + are x, then sum of negative would be -(sum-x)
    - problem requires x - (sum-x) = target  => x = (target + sum) / 2
- Steps for DP table:
    - Define the dp[i][j]:
        - The number of ways to fill a knapsack with a capacity of j (including j) using the elements indexed from [0, i] in nums[i]
    - Define the state transition:
        - **Not Including Item i**: dp[i - 1][j]
        - **Including Item i**: To include item i, you first make room for its capacity. The new knapsack capacity becomes (j - weight of item i). The number of ways to fill the knapsack with this reduced capacity is dp[i - 1][j - nums[i]]
        - **if (nums[i] > j) dp[i][j] = dp[i - 1][j];  else dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i]]**
    - How to initialize the DP array: 
        -  we initiate whole dp as 0 since all are postive integer
        -  dp[0][0] = 1 which means put nothing
        -  first row: dp[0][i] = 0 except only when j = nums[0], then dp = 1
        -  first col: dp[i][0] = 1 except there are multiple numbers = 0 then dp = 2^{# of 0 inside [0,i]}
    - Determine the order of traversal: **from left to right and from top to bottom and those outer inner loop can be changed**
        -  The inener bag loop from large to small:
            -  dp[2] = dp[2 - weight[0]] + value[0] = 15 （dp数组已经都初始化为0）
            -  dp[1] = dp[1 - weight[0]] + value[0] = 15
        -  Only can iterate item first and then bag otherwise each j can only put one item
        -  iterate bag then item: If you loop over j (the capacity) in the outer loop and then iterate over the items i in the inner loop, you are updating dp[j] for the current capacity before considering all items
    - Provide an example to derive the DP array (skip)
-----------
- Steps for DP of rolling array/1D array:
    - Define the dp[j]:
        - dp[j]:The number of ways to fill a knapsack with a capacity of j (including j) 
    - Define the state transition deleting i: dp[j] = dp[j] + dp[j - nums[i]] => **dp[j] += dp[j - nums[i]]**
        - Assume we already have item i, adding ways of diff combination excluding i by dp[j-nums[i]]
        - And we iterate nums i in the outter loop
    - How to initialize the DP array: dp[0] = 1 for putting nothing
    - Determine the order of traversal: **reverse loop for bag and item first then bag**
    - Provide an example to derive the DP array:
        - nums = [1,1,1,1,1] target = 3
        - => bagsize = (5+3)/2 = 4
        - dp = [ [0,15,15,15,15], [0,15,15,20,35], [0,15,15,20,35] ]
```python
# solution 1: dp table
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        total_sum = sum(nums)  # 计算nums的总和
        if abs(target) > total_sum:
            return 0  # 此时没有方案
        if (target + total_sum) % 2 == 1:
            return 0  # 此时没有方案
        target_sum = (target + total_sum) // 2  # 目标和

        # 创建二维动态规划数组，行表示选取的元素数量，列表示累加和
        dp = [[0] * (target_sum + 1) for _ in range(len(nums) + 1)]

        # 初始化状态
        dp[0][0] = 1

        # 动态规划过程
        for i in range(1, len(nums) + 1):
            for j in range(target_sum + 1):
                dp[i][j] = dp[i - 1][j]  # 不选取当前元素
                if j >= nums[i - 1]:
                    dp[i][j] += dp[i - 1][j - nums[i - 1]]  # 选取当前元素

        return dp[len(nums)][target_sum]  # 返回达到目标和的方案数


# solution 2: use rolling arrays / 1D array
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        total_sum = sum(nums)  # 计算nums的总和
        if abs(target) > total_sum:
            return 0  # 此时没有方案
        if (target + total_sum) % 2 == 1:
            return 0  # 此时没有方案
        target_sum = (target + total_sum) // 2  # 目标和
        dp = [0] * (target_sum + 1)  # 创建动态规划数组，初始化为0
        dp[0] = 1  # 当目标和为0时，只有一种方案，即什么都不选
        for num in nums:
            for j in range(target_sum, num - 1, -1):
                dp[j] += dp[j - num]  # 状态转移方程，累加不同选择方式的数量
        return dp[target_sum]  # 返回达到目标和的方案数


```
Time: **O(n^m)**      
Space: **O(n^m)** for solution 1 and **O(n)** for solution 2 


##  LC 474 ones-and-zeroes
[Link](https://leetcode.com/problems/ones-and-zeroes/description/)   
[Cousrse Link](https://programmercarl.com/0474.%E4%B8%80%E5%92%8C%E9%9B%B6.html)
- Two 0/1 bags problem: one for 0 and another for 1
- Steps for DP table:
    - Define the dp[i][j]: The maximum size of the subset of strings with at most i zeros and j ones.
    - Define the state transition: **dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1)**
    - How to initialize the DP array: all 0
    - Determine the order of traversal: **iterate items from left to right twice for m & n**
    - Provide an example to derive the DP array (skip)

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0] * (n + 1) for _ in range(m + 1)]  # 创建二维动态规划数组，初始化为0
        for s in strs:  # 遍历物品
            zeroNum = s.count('0')  # 统计0的个数
            oneNum = len(s) - zeroNum  # 统计1的个数
            for i in range(m, zeroNum - 1, -1):  # 遍历背包容量且从后向前遍历
                for j in range(n, oneNum - 1, -1):
                    dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1)  # 状态转移方程
        return dp[m][n]
```
Time: **O(k*m*n)** k is for string length          
Space: **O(m*n)** 
