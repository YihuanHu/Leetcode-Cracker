# Day45 Dynamic Programming Part13 Subarray#4

##  LC 647 palindromic-substrings
[Link](https://leetcode.com/problems/palindromic-substrings/description/)   
[Cousrse Link](https://programmercarl.com/0647.%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- Steps for DP:
    - Define the dp: dp[i][j] represents the number of occurrences of the subsequence of t ending at **index j-1** within the subsequence of s ending at **index i-1**
    - Define the state transition:     
      - s[i - 1] = t[j - 1]: **dp[i - 1][j - 1]+dp[i - 1][j]** either include t[j] or not
      - s[i - 1] != t[j - 1]: **dp[i][j] = dp[i - 1][j]**
    - How to initialize the DP array: dp[0][j] = dp[i][0] = 0
    - Determine the order of traversal: top to bottom and left to right 
    - Provide an example to derive the DP array: skip
```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        dp = [[0] * (len(t)+1) for _ in range(len(s)+1)]
        for i in range(len(s)):
            dp[i][0] = 1
        for j in range(1, len(t)):
            dp[0][j] = 0
        for i in range(1, len(s)+1):
            for j in range(1, len(t)+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j]
        return dp[-1][-1]
```
Time: **O(n^m)**                                  
Space: **O(n^m)** 

##  LC 583 delete-operation-for-two-strings
[Link](https://leetcode.com/problems/delete-operation-for-two-strings/description/)   
[Cousrse Link](https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.html)
- Compared with LC 115 but we have can edit two strings
- Steps for DP:
    - Define the dp: dp[i][j] represents the minimum number of deletions required to make the substring of word1 ending at **index i-1** equal to the substring of word2 ending at **index j-1**
    - Define the state transition:     
      - word1[i - 1] = word2[j - 1]: **dp[i][j] = dp[i - 1][j - 1]** 
      - word1[i - 1] != word2[j - 1]:
          - Delete word1[i - 1]: dp[i - 1][j] + 1
          - Delete word2[j - 1]: dp[i][j - 1] + 1
          - Delete word1[i - 1] and word2[j - 1]: dp[i - 1][j - 1] + 2
          - Thus, dp[i][j] = min({dp[i - 1][j - 1] + 2, dp[i - 1][j] + 1, dp[i][j - 1] + 1})  
    - How to initialize the DP array: dp[i][0] = i  dp[0][j] = j
    - Determine the order of traversal: top to bottom and left to right 
    - Provide an example to derive the DP array: skip
```python
# solution 1
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0] * (len(word2)+1) for _ in range(len(word1)+1)]
        for i in range(len(word1)+1):
            dp[i][0] = i
        for j in range(len(word2)+1):
            dp[0][j] = j
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1] + 2, dp[i-1][j] + 1, dp[i][j-1] + 1)
        return dp[-1][-1]

# solution 2: LC 1143 using the same way 
class Solution(object):
    def minDistance(self, word1, word2):
        m, n = len(word1), len(word2)
        
        # dp 求解两字符串最长公共子序列
        dp = [[0] * (n+1) for _ in range(m+1)]
        for i in range(1, m+1):
            for j in range(1, n+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
                    
        # 删去最长公共子序列以外元素
        return m + n - 2 * dp[-1][-1]
```
Time: **O(n*m)**                   
Space: **O(n*m)** 

##  LC 72 edit-distance
[Link](https://leetcode.com/problems/edit-distance/description/)   
[Cousrse Link](https://programmercarl.com/0072.%E7%BC%96%E8%BE%91%E8%B7%9D%E7%A6%BB.html)
- Steps for DP table:
    - Define the dp: dp[i][j] represents the minimum edit distance between the substring of word1 ending at **index i-1** and the substring of word2 ending at **index j-1**
    - Define the state transition:     
      - if (word1[i - 1] == word2[j - 1]): dp[i][j] = dp[i - 1][j - 1] (nothing need to edit)
      - if (word1[i - 1] != word2[j - 1])
          - Delete word1[i - 1]: dp[i][j] = dp[i - 1][j] + 1;
          - Delete word2[j - 1]: dp[i][j] = dp[i][j - 1] + 1
          - Replace word1[i-1]: dp[i][j] = dp[i - 1][j - 1] + 1
          - Thus, dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1
    - How to initialize the DP array: dp[i][0] = i  dp[0][j] = j
    - Determine the order of traversal: top to bottom and left to right 
    - Provide an example to derive the DP array: skip

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0] * (len(word2)+1) for _ in range(len(word1)+1)]
        for i in range(len(word1)+1):
            dp[i][0] = i
        for j in range(len(word2)+1):
            dp[0][j] = j
        for i in range(1, len(word1)+1):
            for j in range(1, len(word2)+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1
        return dp[-1][-1]
```
Time: **O(n*m)**                   
Space: **O(n*m)** 


## Adds on
- Subarray Edit Distance Summary [Link](https://programmercarl.com/%E4%B8%BA%E4%BA%86%E7%BB%9D%E6%9D%80%E7%BC%96%E8%BE%91%E8%B7%9D%E7%A6%BB%EF%BC%8C%E5%8D%A1%E5%B0%94%E5%81%9A%E4%BA%86%E4%B8%89%E6%AD%A5%E9%93%BA%E5%9E%AB.html)
