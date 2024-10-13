# Day41 Dynamic Programming Part10 Subarray

##  LC 300 longest-increasing-subsequence
[Link](https://leetcode.com/problems/longest-increasing-subsequence/description/)   
[Cousrse Link](https://programmercarl.com/0300.%E6%9C%80%E9%95%BF%E4%B8%8A%E5%8D%87%E5%AD%90%E5%BA%8F%E5%88%97.html)
- Steps for DP:
    - Define the dp: dp[i] represents the length of the longest increasing subsequence ending at nums[i], including all elements up to and including index i
    - Define the state transition:     
      - The **longest increasing subsequence ending at position `i`** is equal to the **maximum length of all increasing subsequences ending at positions `j` (from 0 to i-1)** that can be extended to include `nums[i]`, plus one (to include `nums[i]` itself).
      -  **if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1)**
    - How to initialize the DP array: all begins with 0
    - Determine the order of traversal: forward 
    - Provide an example to derive the DP array:
        - nums = [0,1,0,3,2]
        - return dp = [1,2,1,3,3] 
```python
# solution 1: dp 
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return len(nums)
        dp = [1] * len(nums)
        result = 1
        for i in range(1, len(nums)):
            for j in range(0, i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
            result = max(result, dp[i]) #取长的子序列
        return result

# solution 2: greedy algo
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return len(nums)
        
        tails = [nums[0]]  # 存储递增子序列的尾部元素
        for num in nums[1:]:
            if num > tails[-1]:
                tails.append(num)  # 如果当前元素大于递增子序列的最后一个元素，直接加入到子序列末尾
            else:
                # 使用二分查找找到当前元素在递增子序列中的位置，并替换对应位置的元素
                left, right = 0, len(tails) - 1
                while left < right:
                    mid = (left + right) // 2
                    if tails[mid] < num:
                        left = mid + 1
                    else:
                        right = mid
                tails[left] = num
        
        return len(tails)  # 返回递增子序列的长度

```
Time: **O(n^2)** for solution 1 and **O(n)** for solution 2                            
Space: **O(n)** 

##  LC 674 longest-continuous-increasing-subsequence
[Link](https://leetcode.com/problems/longest-continuous-increasing-subsequence/description/)   
[Cousrse Link](https://programmercarl.com/0674.%E6%9C%80%E9%95%BF%E8%BF%9E%E7%BB%AD%E9%80%92%E5%A2%9E%E5%BA%8F%E5%88%97.html)
- Like the above LC 300 but the subarray must be continuous so it only cares the prev state while LC 300 cares about 0 to i-1 states
- Steps for DP:
    - Define the dp: dp[i]: Represents the length of the longest continuous increasing subsequence **ending at index i**
    - Define the state transition:     
      - Just check the order strickly ending in i
      - **if (nums[i] > nums[i-1]) dp[i] = dp[i - 1] + 1**
    - How to initialize the DP array: all begins with 0
    - Determine the order of traversal: forward 
    - Provide an example to derive the DP array: skip
```python
# solution 1:DP table
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        result = 1
        dp = [1] * len(nums)
        for i in range(len(nums)-1):
            if nums[i+1] > nums[i]: #连续记录
                dp[i+1] = dp[i] + 1
            result = max(result, dp[i+1])
        return result

# solution 2: greedy algo
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        result = 1 #连续子序列最少也是1
        count = 1
        for i in range(len(nums)-1):
            if nums[i+1] > nums[i]: #连续记录
                count += 1
            else: #不连续，count从头开始
                count = 1
            result = max(result, count)
        return result

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

