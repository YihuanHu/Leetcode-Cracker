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

##  LC 279 perfect-squares
[Link](https://leetcode.com/problems/perfect-squares/description/)   
[Cousrse Link](https://programmercarl.com/0279.%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.html)
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:the min number of getting exact sum j by adding its square sums
    - Define the state transition: dp[j] = min(dp[j - i * i] + 1, dp[j])
    - How to initialize the DP array: dp[0] = 0 and all intiate as INF 
    - Determine the order of traversal: No matter the inner/outer loop or from small to large
    - Provide an example to derive the DP array:
        - `dp[0] = 0`
        - `dp[1] = min(dp[0] + 1) = 1`
        - `dp[2] = min(dp[1] + 1) = 2`
        - `dp[3] = min(dp[2] + 1) = 3`
        - `dp[4] = min(dp[3] + 1, dp[0] + 1) = 1`
        - `dp[5] = min(dp[4] + 1, dp[1] + 1) = 2`

```python
# solution 1:item first
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')] * (n + 1)
        dp[0] = 0

        for i in range(1, int(n ** 0.5) + 1):  # 遍历物品
            for j in range(i * i, n + 1):  # 遍历背包
                # 更新凑成数字 j 所需的最少完全平方数数量
                dp[j] = min(dp[j - i * i] + 1, dp[j])

        return dp[n]

# solution 2: bag first
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')] * (n + 1)
        dp[0] = 0

        for i in range(1, n + 1):  # 遍历背包
            for j in range(1, int(i ** 0.5) + 1):  # 遍历物品
                # 更新凑成数字 i 所需的最少完全平方数数量
                dp[i] = min(dp[i], dp[i - j * j] + 1)

        return dp[n]

```
Time: **O(n * square root of n)**              
Space: **O(n)** 

##  LC 139 word-break
[Link](https://leetcode.com/problems/word-break/description/)   
[Cousrse Link](https://programmercarl.com/0139.%E5%8D%95%E8%AF%8D%E6%8B%86%E5%88%86.html)
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:the min number of getting exact sum j by adding its square sums
    - Define the state transition: dp[j] = min(dp[j - i * i] + 1, dp[j])
    - How to initialize the DP array: dp[0] = 0 and all intiate as INF 
    - Determine the order of traversal: No matter the inner/outer loop or from small to large
    - Provide an example to derive the DP array: 

```python
# solution 1:item first
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')] * (n + 1)
        dp[0] = 0

        for i in range(1, int(n ** 0.5) + 1):  # 遍历物品
            for j in range(i * i, n + 1):  # 遍历背包
                # 更新凑成数字 j 所需的最少完全平方数数量
                dp[j] = min(dp[j - i * i] + 1, dp[j])

        return dp[n]

# solution 2: bag first
class Solution:
    def numSquares(self, n: int) -> int:
        dp = [float('inf')] * (n + 1)
        dp[0] = 0

        for i in range(1, n + 1):  # 遍历背包
            for j in range(1, int(i ** 0.5) + 1):  # 遍历物品
                # 更新凑成数字 i 所需的最少完全平方数数量
                dp[i] = min(dp[i], dp[i - j * j] + 1)

        return dp[n]

```
Time: **O(n * square root of n)**              
Space: **O(n)** 
