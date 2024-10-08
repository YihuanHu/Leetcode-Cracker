# Day37 Dynamic Programming Part6 Unbounded Bag Problem
## Knapsack/Bag Problem
- **combinations: the outer for loop should iterate over the items, and the inner for loop should iterate over the knapsack capacities**
- **permutations: the outer for loop should iterate over the knapsack capacities, and the inner for loop should iterate over the items**
```python
# sample 1
def test_CompletePack():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bagWeight = 4
    dp = [0] * (bagWeight + 1)
    for i in range(len(weight)):  # 遍历物品
        for j in range(weight[i], bagWeight + 1):  # 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
    print(dp[bagWeight])

test_CompletePack()

# sample 2
def test_CompletePack():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bagWeight = 4

    dp = [0] * (bagWeight + 1)

    for j in range(bagWeight + 1):  # 遍历背包容量
        for i in range(len(weight)):  # 遍历物品
            if j - weight[i] >= 0:
                dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

    print(dp[bagWeight])

test_CompletePack()
```

##  LC 322 coin-change
[Link](https://leetcode.com/problems/coin-change/description/)   
[Cousrse Link](https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html)
- Not about combinations or permutations compared with LC 518 for combinations LC 377 for permutations
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:the min number of coins of getting exact amount j
    - Define the state transition: dp[j] = min(dp[j - coins[i]] + 1, dp[j])
    - How to initialize the DP array: dp[0] = 0 and all intiates as INF 
    - Determine the order of traversal: No matter the inner/outer loop or from small to large
    - Provide an example to derive the DP array: skip
```python
# solution 1:item first
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount + 1)  # 创建动态规划数组，初始值为正无穷大
        dp[0] = 0  # 初始化背包容量为0时的最小硬币数量为0

        for coin in coins:  # 遍历硬币列表，相当于遍历物品
            for i in range(coin, amount + 1):  # 遍历背包容量
                if dp[i - coin] != float('inf'):  # 如果dp[i - coin]不是初始值，则进行状态转移
                    dp[i] = min(dp[i - coin] + 1, dp[i])  # 更新最小硬币数量

        if dp[amount] == float('inf'):  # 如果最终背包容量的最小硬币数量仍为正无穷大，表示无解
            return -1
        return dp[amount]  # 返回背包容量为amount时的最小硬币数量

# solution 2: bag first
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount + 1)  # 创建动态规划数组，初始值为正无穷大
        dp[0] = 0  # 初始化背包容量为0时的最小硬币数量为0

        for i in range(1, amount + 1):  # 遍历背包容量
            for j in range(len(coins)):  # 遍历硬币列表，相当于遍历物品
                if i - coins[j] >= 0 and dp[i - coins[j]] != float('inf'):  # 如果dp[i - coins[j]]不是初始值，则进行状态转移
                    dp[i] = min(dp[i - coins[j]] + 1, dp[i])  # 更新最小硬币数量

        if dp[amount] == float('inf'):  # 如果最终背包容量的最小硬币数量仍为正无穷大，表示无解
            return -1
        return dp[amount]  # 返回背包容量为amount时的最小硬币数量
```
Time: **O(n * amount)** n is coin list length               
Space: **O(amount)** 

##  LC 377 combination-sum-iv
[Link](https://leetcode.com/problems/combination-sum-iv/description/)   
[Cousrse Link](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- Compared with the above LC 518
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:number of PERMUTATIONS of getting exact amount j
    - Define the state transition deleting i:
    -   To find the number of ways to fill the knapsack, the formula is: dp[j] += dp[j - nums[i]]
    - How to initialize the DP array: 
        -  dp[0] = 1 otherewise the calculation is meaningless
    - Determine the order of traversal: **weight is the outer loop and item is the inner**
      - Same above
    - Provide an example to derive the DP array:
        -  target = 4, coins = [1, 2, 3]     
        -[  [1, 0, 0, 0, 0],  # Initial state (dp[0] = 1)    
              [1, 1, 0, 0, 0],  # After calculating dp[1] = dp[0]     
              [1, 1, 2, 0, 0],  # After calculating dp[2] = dp[1] + dp[0]     
              [1, 1, 2, 4, 0],  # After calculating dp[3] = dp[2] + dp[1] + dp[0]     
              [1, 1, 2, 4, 7]   # After calculating dp[4] = dp[3] + dp[2] + dp[1]     
            ]      

```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0] * (target + 1)
        dp[0] = 1
        for i in range(1, target + 1):  # 遍历背包
            for j in range(len(nums)):  # 遍历物品
                if i - nums[j] >= 0:
                    dp[i] += dp[i - nums[j]]
        return dp[target]


```
Time: **O(target * n)**  n is nums list length               
Space: **O(target)** 

##  Advanced Climbing Ladder
[Cousrse Link](https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html)
- One example of Permutation
- Compared with LC 57
