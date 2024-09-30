# Day32 Dynamic Programming Part1
## DP
-  DP VS Greedu Algo
    - DP: each state is derived from the previous state
    - Greedy:  directly select the optimal choice from local information
- Steps for DP:
    - Define the dp[i] 
    - Define the state transition 
    - How to initialize the DP array
    - Determine the order of traversal
    - Provide an example to derive the DP array
- How to debug:
    - Did I derive the state transition formula for this problem with an example?
    - Did I print the logs of the DP array?
    - Does the printed DP array match what I expected?
      
## LC 509 fibonacci-number
[Link](https://leetcode.com/problems/fibonacci-number/description/)   
[Cousrse Link](https://programmercarl.com/0509.%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)    
- Steps for DP:
    - Define the dp[i]: The Fibonacci value of the i-th number is dp[i]
    - Define the state transition: problem states dp[i] = dp[i - 1] + dp[i - 2]
    - How to initialize the DP array: problem states dp[0] = 0; dp[1] = 1
    - Determine the order of traversal: dp[i] = dp[i - 1] + dp[i - 2];, we can see that dp[i] depends on dp[i - 1] and dp[i - 2] so must iterate front to back
    - Provide an example to derive the DP array: dp[10] = 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55
```python
# solution 1
class Solution:
    def fib(self, n: int) -> int:
       
        # 排除 Corner Case
        if n == 0:
            return 0
        
        # 创建 dp table 
        dp = [0] * (n + 1)

        # 初始化 dp 数组
        dp[0] = 0
        dp[1] = 1

        # 遍历顺序: 由前向后。因为后面要用到前面的状态
        for i in range(2, n + 1):

            # 确定递归公式/状态转移公式
            dp[i] = dp[i - 1] + dp[i - 2]
        
        # 返回答案
        return dp[n]

# solution 2: use only 2 variables
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1:
            return n
        
        prev1, prev2 = 0, 1
        
        for _ in range(2, n + 1):
            curr = prev1 + prev2
            prev1, prev2 = prev2, curr
        
        return prev2
```
Time: **O(n)**     
Space: **O(n)** for solution 1 and **O(1)** for solution 2


##  LC 738 monotone-increasing-digits
[Link](https://leetcode.com/problems/monotone-increasing-digits/description/)   
[Cousrse Link](https://programmercarl.com/0738.%E5%8D%95%E8%B0%83%E9%80%92%E5%A2%9E%E7%9A%84%E6%95%B0%E5%AD%97.html)
- e.g 332 -> 329 -> 299
- iterate backward
    - One counter example is: Number: 332. If we traverse from front to back, it becomes 329. At this point, 2 is still less than the first digit 3, so the correct result should be 299.    
- Local optimal: Once the condition strNum[i - 1] > strNum[i] occurs (non-monotonically), the first step is to decrease strNum[i - 1] by one and set strNum[i] to 9.
- Global optimal: get max the monotone increasing numbers
```python
class Solution:
    def monotoneIncreasingDigits(self, N: int) -> int:
        # 将整数转换为字符串
        strNum = list(str(N))

        # 从右往左遍历字符串
        for i in range(len(strNum) - 1, 0, -1):
            # 如果当前字符比前一个字符小，说明需要修改前一个字符
            if strNum[i - 1] > strNum[i]:
                strNum[i - 1] = str(int(strNum[i - 1]) - 1)  # 将前一个字符减1
                # 将修改位置后面的字符都设置为9，因为修改前一个字符可能破坏了递增性质
                strNum[i:] = '9' * (len(strNum) - i)

        # 将列表转换为字符串，并将字符串转换为整数并返回
        return int(''.join(strNum))

```
Time: **O(n)**     
Space: **O(n)** 


##  LC 763 partition-labels
[Link](https://leetcode.com/problems/partition-labels/description/)   
[Cousrse Link](https://programmercarl.com/0763.%E5%88%92%E5%88%86%E5%AD%97%E6%AF%8D%E5%8C%BA%E9%97%B4.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)      
- for solution 2, update the the recent last_occurance since any smaller last_occurance gonna be included to avoid one char in several partition
```python
# solution #1 use similar overlapping structure 
class Solution:
    def countLabels(self, s):
        # 初始化一个长度为26的区间列表，初始值为负无穷
        hash = [[float('-inf'), float('-inf')] for _ in range(26)]
        hash_filter = []
        for i in range(len(s)):
            if hash[ord(s[i]) - ord('a')][0] == float('-inf'):
                hash[ord(s[i]) - ord('a')][0] = i
            hash[ord(s[i]) - ord('a')][1] = i
        for i in range(len(hash)):
            if hash[i][0] != float('-inf'):
                hash_filter.append(hash[i])
        return hash_filter

    def partitionLabels(self, s):
        res = []
        hash = self.countLabels(s)
        hash.sort(key=lambda x: x[0])  # 按左边界从小到大排序
        rightBoard = hash[0][1]  # 记录最大右边界
        leftBoard = 0
        for i in range(1, len(hash)):
            if hash[i][0] > rightBoard:  # 出现分割点
                res.append(rightBoard - leftBoard + 1)
                leftBoard = hash[i][0]
            rightBoard = max(rightBoard, hash[i][1])
        res.append(rightBoard - leftBoard + 1)  # 最右端
        return res



# solution 2: use last occurance
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        last_occurrence = {}  # 存储每个字符最后出现的位置
        for i, ch in enumerate(s):
            last_occurrence[ch] = i

        result = []
        start = 0
        end = 0
        for i, ch in enumerate(s):
            end = max(end, last_occurrence[ch])  # 找到当前字符出现的最远位置
            if i == end:  # 如果当前位置是最远位置，表示可以分割出一个区间
                result.append(end - start + 1)
                start = i + 1

        return result
```
Time: **O(n*Logn)**     
Space: **O(n)** 

## Adds on
- [ ] greedy algo summary [Link](https://programmercarl.com/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95%E6%80%BB%E7%BB%93%E7%AF%87.html#%E8%B4%AA%E5%BF%83%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80)
    - No specific cheatsheet to follow
    - Find out local/global locality 
