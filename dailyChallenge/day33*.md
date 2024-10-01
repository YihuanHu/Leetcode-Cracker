# Day33 Dynamic Programming Part2

## LC 62 unique-paths
[Link](https://leetcode.com/problems/unique-paths/)   
[Cousrse Link](https://programmercarl.com/0062.%E4%B8%8D%E5%90%8C%E8%B7%AF%E5%BE%84.html)    
- Steps for DP:
    - Define the dp[i][j]: the number of different paths from (0, 0) to (i, j)
    - Define the state transition: dp[i][j] = dp[i - 1][j] + dp[i][j - 1] we only have 2 ways to reach dp[i][j]
    - How to initialize the DP array: since we can only go down or right, we only one solution for those edges
        - for i in range(m):dp[i][0] = 1
        - for j in range(n):dp[0][j] = 1
    - Determine the order of traversal: we can see that dp[i][j] depends on dp[i - 1] and dp[j - 1] so must iterate front to back
    - Provide an example to derive the DP array: try m,k = 3,3
```python
# solution 1: dp table
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # 创建一个二维列表用于存储唯一路径数
        dp = [[0] * n for _ in range(m)]
        
        # 设置第一行和第一列的基本情况
        for i in range(m):
            dp[i][0] = 1
        for j in range(n):
            dp[0][j] = 1
        
        # 计算每个单元格的唯一路径数
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        
        # 返回右下角单元格的唯一路径数
        return dp[m - 1][n - 1]


# solution 2: use rolling array
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # 创建一个一维列表用于存储每列的唯一路径数
        dp = [1] * n
        
        # 计算每个单元格的唯一路径数
        for j in range(1, m):
            for i in range(1, n):
                dp[i] += dp[i - 1]
        
        # 返回右下角单元格的唯一路径数
        return dp[n - 1]
```
Time: **O(m*n)**     
Space: **O(m*n)** for solution 1 and **O(n)** for solution 2


##  LC unique-paths-ii
[Link](https://leetcode.com/problems/unique-paths-ii/description/)   
[Cousrse Link](https://programmercarl.com/0063.%E4%B8%8D%E5%90%8C%E8%B7%AF%E5%BE%84II.html)
- Steps for DP:
    - Define the dp[i][j]: the number of different paths from (0, 0) to (i, j)
    - Define the state transition:
        - dp[i][j] = dp[i - 1][j] + dp[i][j - 1] we only have 2 ways to reach dp[i][j]
        - **but dp stays 0 for any obstacle and incoming elements** e.g [0 1 0] will lead to [1 0 0] since no way to the very right end
    - How to initialize the DP array: since we can only go down or right, we only one solution for those edges 
        - for i in range(m):dp[i][0] = 1
        - for j in range(n):dp[0][j] = 1
    - Determine the order of traversal: we can see that dp[i][j] depends on dp[i - 1] and dp[j - 1] so must iterate front to back
    - Provide an example to derive the DP array: try m,k = 3,3
```python
# solution 1: dp table
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid):
        m = len(obstacleGrid)  # 网格的行数
        n = len(obstacleGrid[0])  # 网格的列数
        
        if obstacleGrid[m - 1][n - 1] == 1 or obstacleGrid[0][0] == 1:
            # 如果起点或终点有障碍物，直接返回0
            return 0
        
        dp = [[0] * n for _ in range(m)]  # 创建一个二维列表用于存储路径数
        
        # 设置起点的路径数为1
        dp[0][0] = 1 if obstacleGrid[0][0] == 0 else 0
        
        # 计算第一列的路径数
        for i in range(1, m):
            if obstacleGrid[i][0] == 0:
                dp[i][0] = dp[i - 1][0]
        
        # 计算第一行的路径数
        for j in range(1, n):
            if obstacleGrid[0][j] == 0:
                dp[0][j] = dp[0][j - 1]
        
        # 计算其他位置的路径数
        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] == 1:
                    continue
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        
        return dp[m - 1][n - 1]  # 返回终点的路径数

# solution 2: use rolling arrays & break & continue
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid):
        if obstacleGrid[0][0] == 1:
            return 0
        
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        
        dp = [0] * n  # 创建一个一维列表用于存储路径数
        
        # 初始化第一行的路径数
        for j in range(n):
            if obstacleGrid[0][j] == 1:
                break
            dp[j] = 1

        # 计算其他行的路径数
        for i in range(1, m):
            if obstacleGrid[i][0] == 1:
                dp[0] = 0
            for j in range(1, n):
                if obstacleGrid[i][j] == 1:
                    dp[j] = 0
                    continue
                
                dp[j] += dp[j - 1]
        
        return dp[-1]  # 返回最后一个元素，即终点的路径数

```
Time: **O(m*n)**     
Space: **O(m*n)** for solution 1 and **O(n)** for solution 2


##  LC 343 integer-break
[Link](https://leetcode.com/problems/integer-break/description/)   
[Cousrse Link](https://programmercarl.com/0343.%E6%95%B4%E6%95%B0%E6%8B%86%E5%88%86.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- Steps for DP:
    - Define the dp[i]: The maximum product that can be obtained by breaking the number
    - Define the state transition: dp[i] = max({dp[i], (i - j) * j, dp[i - j] * j})
        - there are only two ways of calculation:
        - For only 2 numbers: direct multiplication (i - j) * j
        - More than 2 numbers: dp[i - j] * j
    - How to initialize the DP array: dp[2] = 1 hard to tell dp[0] and dp[1]
    - Determine the order of traversal: we can see that dp[i] depends on dp[i - j] so must iterate front to back. Also, j is the inner loop for each i
    - Provide an example to derive the DP array: for n = 5, dp is [1,2,4,6,9]
```python
# solution 1: dp table
class Solution:
         # 假设对正整数 i 拆分出的第一个正整数是 j（1 <= j < i），则有以下两种方案：
        # 1) 将 i 拆分成 j 和 i−j 的和，且 i−j 不再拆分成多个正整数，此时的乘积是 j * (i-j)
        # 2) 将 i 拆分成 j 和 i−j 的和，且 i−j 继续拆分成多个正整数，此时的乘积是 j * dp[i-j]
    def integerBreak(self, n):
        dp = [0] * (n + 1)   # 创建一个大小为n+1的数组来存储计算结果
        dp[2] = 1  # 初始化dp[2]为1，因为当n=2时，只有一个切割方式1+1=2，乘积为1
       
        # 从3开始计算，直到n
        for i in range(3, n + 1):
            # 遍历所有可能的切割点
            for j in range(1, i // 2 + 1): # mathmaticlly, max multiply only appears when two divides are equal/similar

                # 计算切割点j和剩余部分(i-j)的乘积，并与之前的结果进行比较取较大值
                
                dp[i] = max(dp[i], (i - j) * j, dp[i - j] * j)
        
        return dp[n]  # 返回最终的计算结果



# solution 2: greedy algo and need math prove
class Solution:
    def integerBreak(self, n):
        if n == 2:  # 当n等于2时，只有一种拆分方式：1+1=2，乘积为1
            return 1
        if n == 3:  # 当n等于3时，只有一种拆分方式：2+1=3，乘积为2
            return 2
        if n == 4:  # 当n等于4时，有两种拆分方式：2+2=4和1+1+1+1=4，乘积都为4
            return 4
        result = 1
        while n > 4:
            result *= 3  # 每次乘以3，因为3的乘积比其他数字更大
            n -= 3  # 每次减去3
        result *= n  # 将剩余的n乘以最后的结果
        return result
```
Time: **O(n^2)** for solution 1 and **O(n)** for solution 2
Space: **O(n)** for solution 1 and **O(1)** for solution 2


## * LC 96 unique-binary-search-trees
[Link](https://leetcode.com/problems/unique-binary-search-trees/description/)   
[Cousrse Link](https://programmercarl.com/0096.%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html)
- Steps for DP:
    - Define the dp[i]: The maximum product that can be obtained by breaking the number
    - Define the state transition: dp[i] = max({dp[i], (i - j) * j, dp[i - j] * j})
        - there are only two ways of calculation:
        - For only 2 numbers: direct multiplication (i - j) * j
        - More than 2 numbers: dp[i - j] * j
    - How to initialize the DP array: dp[2] = 1 hard to tell dp[0] and dp[1]
    - Determine the order of traversal: we can see that dp[i] depends on dp[i - j] so must iterate front to back. Also, j is the inner loop for each i
    - Provide an example to derive the DP array: for n = 5, dp is [1,2,4,6,9]
```python
# solution 1: dp table
class Solution:
         # 假设对正整数 i 拆分出的第一个正整数是 j（1 <= j < i），则有以下两种方案：
        # 1) 将 i 拆分成 j 和 i−j 的和，且 i−j 不再拆分成多个正整数，此时的乘积是 j * (i-j)
        # 2) 将 i 拆分成 j 和 i−j 的和，且 i−j 继续拆分成多个正整数，此时的乘积是 j * dp[i-j]
    def integerBreak(self, n):
        dp = [0] * (n + 1)   # 创建一个大小为n+1的数组来存储计算结果
        dp[2] = 1  # 初始化dp[2]为1，因为当n=2时，只有一个切割方式1+1=2，乘积为1
       
        # 从3开始计算，直到n
        for i in range(3, n + 1):
            # 遍历所有可能的切割点
            for j in range(1, i // 2 + 1): # mathmaticlly, max multiply only appears when two divides are equal/similar

                # 计算切割点j和剩余部分(i-j)的乘积，并与之前的结果进行比较取较大值
                
                dp[i] = max(dp[i], (i - j) * j, dp[i - j] * j)
        
        return dp[n]  # 返回最终的计算结果



# solution 2: greedy algo and need math prove
class Solution:
    def integerBreak(self, n):
        if n == 2:  # 当n等于2时，只有一种拆分方式：1+1=2，乘积为1
            return 1
        if n == 3:  # 当n等于3时，只有一种拆分方式：2+1=3，乘积为2
            return 2
        if n == 4:  # 当n等于4时，有两种拆分方式：2+2=4和1+1+1+1=4，乘积都为4
            return 4
        result = 1
        while n > 4:
            result *= 3  # 每次乘以3，因为3的乘积比其他数字更大
            n -= 3  # 每次减去3
        result *= n  # 将剩余的n乘以最后的结果
        return result
```
Time: **O(n^2)** for solution 1 and **O(n)** for solution 2
Space: **O(n)** for solution 1 and **O(1)** for solution 2
