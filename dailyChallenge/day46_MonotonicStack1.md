# Day46 Monotonic Stack #1
## Monotonic Stack
- when to use monotonic stack?
  - **when dealing with a one-dimensional array and we need to find the position of the first element to the right or left that is either larger or smaller than the current element**
- Time complexity is O(n).
  - Sacrifice space for quick time. We use a stack to keep track of the elements we’ve traversed. When we iterate through the array, we don’t know which elements we’ve previously encountered
- What elements should be stored in the monotonic stack?
  - The monotonic stack only needs to store the index i of the element and directly access it with T[i]
- Should the elements in the stack be in increasing or decreasing order?
  - **from the top of the stack to the bottom**, if we are looking for the next greater element to the right, the monotonic stack should be increasing
- Logic for creating the M stack: current element T[i] and the top element T[st.top()]
  -  If T[i] < T[st.top()]
  -  If T[i] = T[st.top()]
  -  If T[i] > T[st.top()]

##  LC 739 daily-temperatures
[Link](https://leetcode.com/problems/daily-temperatures/description/)   
[Cousrse Link](https://programmercarl.com/0739.%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.html)
- Use increaseing stack (from top to bottom)
  -  If T[i] < T[st.top()], add to stack
  -  If T[i] = T[st.top()], add to stack
  -  If T[i] > T[st.top()], pop and calculate res till elements inside is larger than top

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        answer = [0]*len(temperatures)
        stack = [0]
        for i in range(1,len(temperatures)):
            # 情况一和情况二
            if temperatures[i]<=temperatures[stack[-1]]:
                stack.append(i)
            # 情况三
            else:
                while len(stack) != 0 and temperatures[i]>temperatures[stack[-1]]:
                    answer[stack[-1]]=i-stack[-1]
                    stack.pop()
                stack.append(i)

        return answer
```
Time: **O(n)**                                  
Space: **O(n)** 

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
