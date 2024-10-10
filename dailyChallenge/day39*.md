# Day39 Dynamic Programming Part8

##  LC 121 best-time-to-buy-and-sell-stock
[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)   
[Cousrse Link](https://programmercarl.com/0121.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BA.html)
- Greedy Algo: find the smallest on the left and largest on the right
- Steps for DP:
    - Define the dp [i][0] and dp[i][1]:
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

##  LC 123 best-time-to-buy-and-sell-stock-iii
[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/description/)   
[Cousrse Link](https://programmercarl.com/0123.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAIII.html)
- Steps for DP:
    - Define the dp:
        - `dp[i][0]` represents the maximum cash obtained on day `i` when **No operation** (we could actually omit this state).
        - `dp[i][1]` represents the maximum cash obtained on day `i` when **first time holding a stock**.
        - `dp[i][2]` represents the maximum cash obtained on day `i` when **first time not holding a stock**.
        - `dp[i][3]` represents the maximum cash obtained on day `i` when **second time holding a stock**.
        - `dp[i][4]` represents the maximum cash obtained on day `i` when **second time not holding a stock**.
    - Define the state transition:
    - If hold a stock at day i , then we can either hold beofre day i or buy at day i:  dp[i][1] = max(dp[i-1][0] - prices[i], dp[i - 1][1])
    - If did not hold a stock at day i, then we can either sell it before day i or sell at day i:  dp[i][2] = max(dp[i - 1][1] + prices[i], dp[i - 1][2])
    - same for 3 & 4: dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i]); dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i]);
    - How to initialize the DP array:
      - dp[0][0] = 0
      - dp[0][1] = -prices[0]
      - dp[0][2] = 0
      - dp[0][3] = -prices[0]
      - dp[0][4] = 0
    - Determine the order of traversal: forward since [i-1] 
    - Provide an example to derive the DP array: skip

```python
# solution 1: dp table
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        dp = [[0] * 5 for _ in range(len(prices))]
        dp[0][1] = -prices[0]
        dp[0][3] = -prices[0]
        for i in range(1, len(prices)):
            dp[i][0] = dp[i-1][0]
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
            dp[i][2] = max(dp[i-1][2], dp[i-1][1] + prices[i])
            dp[i][3] = max(dp[i-1][3], dp[i-1][2] - prices[i])
            dp[i][4] = max(dp[i-1][4], dp[i-1][3] + prices[i])
        return dp[-1][4]

# solution 2: 1 d array 
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices) == 0:
            return 0
        dp = [0] * 5 
        dp[1] = -prices[0]
        dp[3] = -prices[0]
        for i in range(1, len(prices)):
            dp[1] = max(dp[1], dp[0] - prices[i])
            dp[2] = max(dp[2], dp[1] + prices[i])
            dp[3] = max(dp[3], dp[2] - prices[i])
            dp[4] = max(dp[4], dp[3] + prices[i])
        return dp[4]
```
Time: **O(n)**              
Space: **O(n)** 

