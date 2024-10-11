# Day40 Dynamic Programming Part9 Stock#3

##  LC 188 best-time-to-buy-and-sell-stock-iv
[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)   
[Cousrse Link](https://programmercarl.com/0188.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAIV.html)
- Similar to LC 123
- Steps for DP:
    - Define the dp (odd are holding while even are not holding)
        - `dp[i][0]` represents the maximum cash obtained on day `i` when **No operation** (we could actually omit this state).
        - `dp[i][1]` represents the maximum cash obtained on day `i` when **first time holding a stock**.
        - `dp[i][2]` represents the maximum cash obtained on day `i` when **first time not holding a stock** etc.
    - Define the state transition:     
      - If hold a stock at day i(odd) , then we can either hold beofre day i or buy at day i:  dp[i][j + 1] = max(dp[i - 1][j + 1], dp[i - 1][j] - prices[i])
      - If did not hold a stock at day i(even), then we can either sell it before day i or sell at day i: dp[i][j + 2] = max(dp[i - 1][j + 2], dp[i - 1][j + 1] + prices[i])
    - How to initialize the DP array:
      - for odd: dp[0][0] = 0  dp[0][2] = 0  dp[0][4] = 0
      - for even: dp[0][1] = -prices[0]  dp[0][3] = -prices[0]
    - Determine the order of traversal: forward since [i-1] 
    - Provide an example to derive the DP array: skip
```python
# solution 1:greedy algo
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        dp = [[0] * (2*k+1) for _ in range(len(prices))]
        for j in range(1, 2*k, 2):
            dp[0][j] = -prices[0]
        for i in range(1, len(prices)):
            for j in range(0, 2*k-1, 2):
                dp[i][j+1] = max(dp[i-1][j+1], dp[i-1][j] - prices[i])
                dp[i][j+2] = max(dp[i-1][j+2], dp[i-1][j+1] + prices[i])
        return dp[-1][2*k]

# solution 2: dp table
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        if len(prices) == 0: return 0
        dp = [0] * (2*k + 1)
        for i in range(1,2*k,2):
            dp[i] = -prices[0]
        for i in range(1,len(prices)):
            for j in range(1,2*k + 1):
                if j % 2:
                    dp[j] = max(dp[j],dp[j-1]-prices[i])
                else:
                    dp[j] = max(dp[j],dp[j-1]+prices[i])
        return dp[2*k]
```
Time: **O(n*k)**                   
Space: **O(n*k)** for solution 1 and **O(n)** for solution 2

##  LC 309 best-time-to-buy-and-sell-stock-with-cooldown
[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)   
[Cousrse Link](https://programmercarl.com/0309.%E6%9C%80%E4%BD%B3%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E6%97%B6%E6%9C%BA%E5%90%AB%E5%86%B7%E5%86%BB%E6%9C%9F.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- Like the above LC 122 but we have a cool down and thus we need a seperate state for not holding for selling today to distinguishi cool down
- Steps for DP:
    - Define the dp:
        - `dp[i][0]` represents the maximum cash obtained on day `i` **Holding a stock** (either bought the stock today, or bought it earlier and have been holding without any action
        - `dp[i][1]` represents the maximum cash obtained on day `i` **Maintaining the state of having sold the stock** (sold the stock two days ago and completed the one-day cooldown, or had already sold the stock before and did nothing further
        - `dp[i][2]` represents the maximum cash obtained on day `i` when **Selling the stock today**.
        - `dp[i][3]` represents the maximum cash obtained on day `i` when **Today is a cooldown state**.
    - Define the state transition:     
      - If hold a stock at day i, then we can either hold beofre day i or buy at day i(yesterday is cooling day/complete cooldown long before):  dp[i][0] = max(dp[i - 1][0], dp[i - 1][3] - prices[i], dp[i - 1][1] - prices[i]);
      - If did not hold a stock at day i, then we can either sell it long before day i or sell it yesterday: dp[i][1] = max(dp[i - 1][1], dp[i - 1][3])
      - If selling the stock at day i, then we hold a stock in yesterday: dp[i][2] = dp[i - 1][0] + prices[i]
      - If today i is a cooldown, then we must sell it yesterday: dp[i][3] = dp[i - 1][2] 
    - How to initialize the DP array:
      - dp[0][0] = -prices[0]
      - and all other dp[0][j] are all 0
    - Determine the order of traversal: forward since [i-1] 
    - Provide an example to derive the DP array:
      
| Day (Index) | Stock Price | State 0 | State 1 | State 2 | State 3 |
|-------------|-------------|---------|---------|---------|---------|
| 1           | 1           | -1      | 0       | 0       | 0       |
| 2           | 2           | -1      | 0       | 1       | 0       |
| 3           | 3           | -1      | 0       | 2       | 1       |
| 4           | 0           | 1       | 1       | -1      | 2       |
| 5           | 2           | 1       | 2       | 3       | -1      |
```python
# solution 1:DP table
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0:
            return 0
        dp = [[0] * 4 for _ in range(n)]  # 创建动态规划数组，4个状态分别表示持有股票、不持有股票且处于冷冻期、不持有股票且不处于冷冻期、不持有股票且当天卖出后处于冷冻期
        dp[0][0] = -prices[0]  # 初始状态：第一天持有股票的最大利润为买入股票的价格
        for i in range(1, n):
            dp[i][0] = max(dp[i-1][0], max(dp[i-1][3], dp[i-1][1]) - prices[i])  # 当前持有股票的最大利润等于前一天持有股票的最大利润或者前一天不持有股票且不处于冷冻期的最大利润减去当前股票的价格
            dp[i][1] = max(dp[i-1][1], dp[i-1][3])  # 当前不持有股票且处于冷冻期的最大利润等于前一天持有股票的最大利润加上当前股票的价格
            dp[i][2] = dp[i-1][0] + prices[i]  # 当前不持有股票且不处于冷冻期的最大利润等于前一天不持有股票的最大利润或者前一天处于冷冻期的最大利润
            dp[i][3] = dp[i-1][2]  # 当前不持有股票且当天卖出后处于冷冻期的最大利润等于前一天不持有股票且不处于冷冻期的最大利润
        return max(dp[n-1][3], dp[n-1][1], dp[n-1][2])  # 返回最后一天不持有股票的最大利润


# solution 2: 1D array
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n < 2:
            return 0

        # 定义三种状态的动态规划数组
        dp = [[0] * 3 for _ in range(n)]
        dp[0][0] = -prices[0]  # 持有股票的最大利润
        dp[0][1] = 0           # 不持有股票，且处于冷冻期的最大利润
        dp[0][2] = 0           # 不持有股票，不处于冷冻期的最大利润

        for i in range(1, n):
            # 当前持有股票的最大利润等于前一天持有股票的最大利润或者前一天不持有股票且不处于冷冻期的最大利润减去当前股票的价格
            dp[i][0] = max(dp[i-1][0], dp[i-1][2] - prices[i])
            # 当前不持有股票且处于冷冻期的最大利润等于前一天持有股票的最大利润加上当前股票的价格
            dp[i][1] = dp[i-1][0] + prices[i]
            # 当前不持有股票且不处于冷冻期的最大利润等于前一天不持有股票的最大利润或者前一天处于冷冻期的最大利润
            dp[i][2] = max(dp[i-1][2], dp[i-1][1])

        # 返回最后一天不持有股票的最大利润
        return max(dp[-1][1], dp[-1][2])

```
Time: **O(n)**                   
Space: **O(n)** for solution 1 and **O(1)** for solution 2

##  LC 714 best-time-to-buy-and-sell-stock-with-transaction-fee
[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)   
[Cousrse Link](https://programmercarl.com/0714.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA%E5%90%AB%E6%89%8B%E7%BB%AD%E8%B4%B9%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html)
- Similar to LC 122 but with a transaction fee
- Define the state transition:     
  - If hold a stock at day i , then we can either hold beofre day i or buy at day i:  dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i])
  - If did not hold a stock at day i/even, then we can either sell it before day i or sell at day i **need to pay the fee**: dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]-fee)


```python
# solution 1: dp table
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)
        dp = [[0] * 2 for _ in range(n)]
        dp[0][0] = -prices[0] #持股票
        for i in range(1, n):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] - prices[i])
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] + prices[i] - fee)
        return max(dp[-1][0], dp[-1][1])

# solution 2: 1 d array 
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        # 持有股票手上的最大現金
        hold = -prices[0] - fee
        # 不持有股票手上的最大現金
        not_hold = 0
        for price in prices[1:]:
            new_hold = max(hold, not_hold - price - fee)
            new_not_hold = max(not_hold, hold + price)
            hold, not_hold = new_hold, new_not_hold
        return not_hold
```
Time: **O(n)**              
Space: **O(n)** for solution 1 and **O(1)** for solution 2

## Adds On
- Summary for stock type: [Link](https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92-%E8%82%A1%E7%A5%A8%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93%E7%AF%87.html#%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAii)

