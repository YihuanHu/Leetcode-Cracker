# Day44 Dynamic Programming Part12 Subarray#3

##  LC 115 distinct-subsequences
[Link](https://leetcode.com/problems/distinct-subsequences/description/)   
[Cousrse Link](https://programmercarl.com/0115.%E4%B8%8D%E5%90%8C%E7%9A%84%E5%AD%90%E5%BA%8F%E5%88%97.html)
- Steps for DP:
    - Define the dp: dp[i][j] represents the length of the longest common subsequence (LCS) between the substring of text1 of length **[0, i-1]** and the substring of text2 of **length [0, j-1]**
    - Define the state transition:     
      - **if text1[i - 1] == text2[j - 1]: dp[i][j] = dp[i - 1][j - 1] + 1**
      - **else: dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])**
    - How to initialize the DP array: all begins with 0
    - Determine the order of traversal: top to bottom and left to right 
    - Provide an example to derive the DP array: skip
```python
# solution 1: dp table
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        # 创建一个二维数组 dp，用于存储最长公共子序列的长度
        dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]
        
        # 遍历 text1 和 text2，填充 dp 数组
        for i in range(1, len(text1) + 1):
            for j in range(1, len(text2) + 1):
                if text1[i - 1] == text2[j - 1]:
                    # 如果 text1[i-1] 和 text2[j-1] 相等，则当前位置的最长公共子序列长度为左上角位置的值加一
                    dp[i][j] = dp[i - 1][j - 1] + 1
                else:
                    # 如果 text1[i-1] 和 text2[j-1] 不相等，则当前位置的最长公共子序列长度为上方或左方的较大值
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
        
        # 返回最长公共子序列的长度
        return dp[len(text1)][len(text2)]

# solution 2: 1D array
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        m, n = len(text1), len(text2)
        dp = [0] * (n + 1)  # 初始化一维DP数组
        
        for i in range(1, m + 1):
            prev = 0  # 保存上一个位置的最长公共子序列长度
            for j in range(1, n + 1):
                curr = dp[j]  # 保存当前位置的最长公共子序列长度
                if text1[i - 1] == text2[j - 1]:
                    # 如果当前字符相等，则最长公共子序列长度加一
                    dp[j] = prev + 1
                else:
                    # 如果当前字符不相等，则选择保留前一个位置的最长公共子序列长度中的较大值
                    dp[j] = max(dp[j], dp[j - 1])
                prev = curr  # 更新上一个位置的最长公共子序列长度
        
        return dp[n]  # 返回最后一个位置的最长公共子序列长度作为结果


```
Time: **O(n^m)**                                  
Space: **O(n^m)** for solution 1 and **O(m)** for solution 2

##  LC 1035 uncrossed-lines
[Link](https://leetcode.com/problems/uncrossed-lines/description/)   
[Cousrse Link](https://programmercarl.com/1035.%E4%B8%8D%E7%9B%B8%E4%BA%A4%E7%9A%84%E7%BA%BF.html)
- The same as the above LC 1143:
    - The lines cannot intersect, which means that in string A, you need to find a subsequence that matches string B, and this subsequence must maintain the relative order. As long as the relative order is not changed, the lines connecting the same characters will not intersect =>  This common subsequence refers to the relative order being maintained
```python
class Solution:
    def maxUncrossedLines(self, A: List[int], B: List[int]) -> int:
        dp = [[0] * (len(B)+1) for _ in range(len(A)+1)]
        for i in range(1, len(A)+1):
            for j in range(1, len(B)+1):
                if A[i-1] == B[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return dp[-1][-1]

```
Time: **O(n*m)**                   
Space: **O(n*m)** 

##  LC 53 maximum-subarray
[Link](https://leetcode.com/problems/maximum-subarray/description/)   
[Cousrse Link](https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html)
- Steps for DP table:
    - Define the dp: dp[i] represents the maximum sum of a contiguous subarray that **ends at index i** 
    - Define the state transition: **dp[i] = max(dp[i - 1] + nums[i], nums[i])**
        - Starting the new sums at i: nums[i]
        - Continue the sum from prev: dp[i - 1] + nums[i] 
    - How to initialize the DP array: dp[0] = nums[0]
    - Determine the order of traversal: forward 
    - Provide an example to derive the DP array:
        - nums = [-2,1,-3,4,-1,2,1,-5,4]
        - dp = [-2,1,-2,4,3,5,6,1,5]

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [0] * len(nums)
        dp[0] = nums[0]
        result = dp[0]
        for i in range(1, len(nums)):
            dp[i] = max(dp[i-1] + nums[i], nums[i]) #状态转移公式
            result = max(result, dp[i]) #result 保存dp[i]的最大值
        return result
```
Time: **O(n)**              
Space: **O(n)** 


##  LC 392 is-subsequence
[Link](https://leetcode.com/problems/is-subsequence/description/)   
[Cousrse Link](https://programmercarl.com/0392.%E5%88%A4%E6%96%AD%E5%AD%90%E5%BA%8F%E5%88%97.html)
- Similar to LC 1143
- Steps for DP table:
    - Define the dp: dp[i][j] represents the length of the **longest common subsequence** between the substring of s **ending at index i-1** and the substring of t **ending at index j-1**
    - Define the state transition: 
        - if (s[i - 1] == t[j - 1]): dp[i][j] = dp[i - 1][j - 1] + 1
        - if (s[i - 1] != t[j - 1]): dp[i][j] = dp[i][j - 1]  
    - How to initialize the DP array:
        - initiate first row&col as 0: dp[i][0] = dp[0][j] = 0
    - Determine the order of traversal: top to bottom and left to right 
    - Provide an example to derive the DP array:

|   |   | a | h | b | g | d | c |
|---|---|---|---|---|---|---|---|
|   | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| a | 0 | 1 | 1 | 1 | 1 | 1 | 1 |
| b | 0 | 0 | 0 | 2 | 2 | 2 | 2 |
| c | 0 | 0 | 0 | 0 | 0 | 0 | 3 |


```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        dp = [[0] * (len(t)+1) for _ in range(len(s)+1)]
        for i in range(1, len(s)+1):
            for j in range(1, len(t)+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = dp[i][j-1]
        if dp[-1][-1] == len(s):
            return True
        return False
```
Time: **O(n*m)**              
Space: **O(n*m)** 
