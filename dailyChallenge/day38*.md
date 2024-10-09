# Day38 Dynamic Programming Part7
## Unbounded Knapsack/Bag Problem
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
## bounded Knapsack/Bag Problem
- generalization of the 0/1 knapsack problem where each item has a limit on the number of times
- Add one more loop inside the item to go through the counts for each item

##  LC 322 coin-change
[Link](https://leetcode.com/problems/coin-change/description/)   
[Cousrse Link](https://programmercarl.com/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.html)
- Not about combinations or permutations compared with LC 518 for combinations LC 377 for permutations
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:the min number of coins of getting exact amount j
    - Define the state transition: dp[j] = min(dp[j - coins[i]] + 1, dp[j])
    - How to initialize the DP array: dp[0] = 0 and all other intiates as INF 
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
    - How to initialize the DP array: dp[0] = 0 and all other intiate as INF 
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
- We can use backtracking and memoried recursion like LC 131 but its Time Complexity is O(2 ^ n )         
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:If the length of the string is i, dp[i] being true means that the string can be split into one or more words that appear in the dictionary
    - Define the state transition:
        - If the substring in the range [j, i] appears in the dictionary & dp[j] is true, then dp[i] is set to true
    - How to initialize the DP array: dp[0] = True and all other initiate as False 
    - Determine the order of traversal:
        - Try to get the permutation since order matters
        - so bag first and item second 
    - Provide an example to derive the DP array:
        - String: `"leetcode"`
        - Dictionary: `["leet", "code"]`
         
| Index (i) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-----------|---|---|---|---|---|---|---|---|---|
| `dp[i]`   | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 1 |


```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        wordSet = set(wordDict)
        n = len(s)
        dp = [False] * (n + 1)  # dp[i] 表示字符串的前 i 个字符是否可以被拆分成单词
        dp[0] = True  # 初始状态，空字符串可以被拆分成单词

        for i in range(1, n + 1): # 遍历背包
            for j in range(i): # 遍历单词
                if dp[j] and s[j:i] in wordSet:
                    dp[i] = True  # 如果 s[0:j] 可以被拆分成单词，并且 s[j:i] 在单词集合中存在，则 s[0:i] 可以被拆分成单词
                    break

        return dp[n]
```
Time: **O(n ^ 2)**              
Space: **O(n)** 


## Adds On
- Topics summary for Bag:
- For state transition:
    ### 问题 1: 能否装满背包（或者最多装多少）
    公式：`dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);`  
    对应题目如下：
    - 动态规划：416. 分割等和子集
    - 动态规划：1049. 最后一块石头的重量 II
    
    ### 问题 2: 装满背包有几种方法
    公式：`dp[j] += dp[j - nums[i]]`  
    对应题目如下：
    - 动态规划：494. 目标和
    - 动态规划：518. 零钱兑换 II
    - 动态规划：377. 组合总和 IV
    - 动态规划：70. 爬楼梯进阶版（完全背包）
    
    ### 问题 3: 装满背包的最大价值
    公式：`dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);`  
    对应题目如下：
    - 动态规划：474. 一和零
    
    ### 问题 4: 装满背包所有物品的最小个数
    公式：`dp[j] = min(dp[j - coins[i]] + 1, dp[j]);`  
    对应题目如下：
    - 动态规划：322. 零钱兑换
    - 动态规划：279. 完全平方数
- For iteration:
    ### 01 背包
    在 [动态规划：关于 01 背包问题，你该了解这些！] 中，我们讲解了**二维 dp 数组的 01 背包**，无论是先遍历物品还是先遍历背包容量都是可以的，且第二层 `for` 循环是从小到大遍历。
    
    在 [动态规划：关于 01 背包问题，你该了解这些！（滚动数组）] 中，我们讲解了**一维 dp 数组的 01 背包**。在这种情况下，必须**先遍历物品，再遍历背包容量**，且第二层 `for` 循环是从**大到小**遍历。
    
    ### 完全背包
    在 [动态规划：关于完全背包，你该了解这些！] 中，介绍了**纯完全背包的一维 dp 数组实现**，在这种情况下，无论是先遍历物品还是先遍历背包容量都是可以的，且第二层 `for` 循环是从**小到大**遍历。
    
    但仅限于**纯完全背包**的问题，这种遍历顺序是适用的。如果题目稍有变化，两个 `for` 循环的先后顺序可能就不同了：
    
    - **求组合数**时，外层 `for` 循环遍历**物品**，内层 `for` 循环遍历**背包容量**。
    - **求排列数**时，外层 `for` 循环遍历**背包容量**，内层 `for` 循环遍历**物品**。
    
    #### 相关题目
    - **求组合数**：
      - 动态规划：518. 零钱兑换 II
    
    - **求排列数**：
      - 动态规划：377. 组合总和 IV
      - 动态规划：70. 爬楼梯进阶版（完全背包）
    
    - **求最小数**：
      - 如果求最小数，两层 `for` 循环的顺序没有特别要求，相关题目如下：
        - 动态规划：322. 零钱兑换
        - 动态规划：279. 完全平方数

- Bag summary [Link](https://programmercarl.com/%E8%83%8C%E5%8C%85%E6%80%BB%E7%BB%93%E7%AF%87.html)
- Table summary:       

| Problem Type         | Inner Loop  | Iteration for Inner Loop  |
|----------------------|-------------|---------------------------|
| 0/1 knapsack (2D dp) | Item/Bag    | Small to large            |
| 0/1 knapsack (1D dp) | Bag         | Large to small            |
| Unbounded knapsack (2D dp) | Item/Bag | Small to large         |
| Unbounded knapsack (1D dp) | Item/Bag | Small to large         |
