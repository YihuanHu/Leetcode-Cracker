# Day45 Dynamic Programming Part13 Subarray#4

##  LC 647 palindromic-substrings
[Link](https://leetcode.com/problems/palindromic-substrings/description/)   
[Cousrse Link](https://programmercarl.com/0647.%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- Steps for DP:
    - * Define the dp: dp[i][j] indicates whether the substring in the range [i, j] (inclusive on both ends) is a palindrome. If it is, dp[i][j] is true; otherwise, it is false
    - Define the state transition:     
      - If s[i] != s[j]: FALSE
      - If s[i] = s[j]:
          - Case 1: When indices i and j are the same, it's the same character (e.g., 'a'), which is of course a palindrome.
          - Case 2: When the difference between indices i and j is 1 (e.g., 'aa'), it is also a palindrome.
          - Case 3: When the difference between indices i and j is greater than 1 (e.g., 'cabac'), if s[i] and s[j] are the same, we need to check whether the substring between i and j is a palindrome by checking if the substring aba is a palindrome. The range of aba is between i+1 and j-1, so we can determine if it is a palindrome by checking if dp[i + 1][j - 1] is true.
    - How to initialize the DP array: all FALSE
    - Determine the order of traversal: we need dp[i + 1][j - 1] so **bottom to top and left to right**
    - Provide an example to derive the DP array:
        - input = 'aaa'
        - **we only fill the upper right part since i < j**
          
| a  | a  |  a |
|---|---|---|
| 1 | 1 | 1 |
| 0 | 1 | 1 |
| 0 | 0 | 1 |

```python
# solution 1: dp
class Solution:
    def countSubstrings(self, s: str) -> int:
        dp = [[False] * len(s) for _ in range(len(s))]
        result = 0
        for i in range(len(s)-1, -1, -1): #注意遍历顺序
            for j in range(i, len(s)):
                if s[i] == s[j]:
                    if j - i <= 1: #情况一 和 情况二
                        result += 1
                        dp[i][j] = True
                    elif dp[i+1][j-1]: #情况三
                        result += 1
                        dp[i][j] = True
        return result

# solution 2: two pointer
class Solution:
    def countSubstrings(self, s: str) -> int:
        result = 0
        for i in range(len(s)):
            result += self.extend(s, i, i, len(s)) #以i为中心
            result += self.extend(s, i, i+1, len(s)) #以i和i+1为中心
        return result
    
    def extend(self, s, i, j, n):
        res = 0
        while i >= 0 and j < n and s[i] == s[j]:
            i -= 1
            j += 1
            res += 1
        return res
```
Time: **O(n^2)**                                  
Space: **O(n^2)** for solution 1 and **O(1)** for solution 1 

##  LC 516 longest-palindromic-subsequence
[Link](https://leetcode.com/problems/longest-palindromic-subsequence/description/)   
[Cousrse Link](https://programmercarl.com/0516.%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E5%BA%8F%E5%88%97.html)
- This is similar to the above LC 647 but it is subseuqence(can delete) while LC 647 is the substrings(exact match)
- Steps for DP:
    - Define the dp: dp[i][j] represents the length of the longest palindromic subsequence in the substring of s within the range [i, j]
    - Define the state transition:     
      - If s[i] != s[j]: dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
          - Case 1: Include i, dp[i][j - 1]
          - Case 2: Include j, dp[i + 1][j]
      - If s[i] = s[j]: dp[i][j] = dp[i + 1][j - 1] + 2
    - How to initialize the DP array: all 0 and **dp[i][j] = 1 when i=j**
    - Determine the order of traversal: we need dp[i + 1][j - 1] so **bottom to top and left to right**
    - Provide an example to derive the DP array:
        - inputs = 'cbbd' 
 
|   | c | b | b | d |
|---|---|---|---|---|
| c | 1 | 1 | 2 | 2 |
| b | 0 | 1 | 2 | 2 |
| b | 0 | 0 | 1 | 1 |
| d | 0 | 0 | 0 | 1 |

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        dp = [[0] * len(s) for _ in range(len(s))]
        for i in range(len(s)):
            dp[i][i] = 1
        for i in range(len(s)-1, -1, -1):
            for j in range(i+1, len(s)):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])
        return dp[0][-1]
```
Time: **O(n*2)**                   
Space: **O(n*2)** 

## Adds on
- DP Summary [Link](https://programmercarl.com/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E6%80%BB%E7%BB%93%E7%AF%87.html)
    - basic
    - bag
    - steal house
    - stock
    - subarray  
