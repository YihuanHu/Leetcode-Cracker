# Day41 Dynamic Programming Part10 Subarray

##  LC 1143 longest-common-subsequence
[Link](https://leetcode.com/problems/longest-common-subsequence/description/)   
[Cousrse Link](https://programmercarl.com/1143.%E6%9C%80%E9%95%BF%E5%85%AC%E5%85%B1%E5%AD%90%E5%BA%8F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
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

##  LC 718 maximum-length-of-repeated-subarray
[Link](https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/)   
[Cousrse Link](https://programmercarl.com/0718.%E6%9C%80%E9%95%BF%E9%87%8D%E5%A4%8D%E5%AD%90%E6%95%B0%E7%BB%84.html)
- Steps for DP table:
    - Define the dp:
        - dp[i]: dp[i][j] represents the length of the longest common subarray ending at index **i - 1** in A and index **j - 1** in B
        - why not use i for A and j for B: to avoid initialize the first row and col for dp
    - Define the state transition: **if nums1[i - 1] == nums2[j - 1]: dp[i][j] = dp[i - 1][j - 1] + 1**
    - How to initialize the DP array: all 0 
    - Determine the order of traversal: forward and no matter the inner/outer
    - Provide an example to derive the DP array:
        - A = [1,2,3,2,1] B = [3,2,1,4,7]

| A \ B  | 3 | 2 | 1 | 4 | 7 |
|--------|---|---|---|---|---|
| **1**  | 0 | 0 | 1 | 0 | 0 |
| **2**  | 0 | 1 | 0 | 0 | 0 |
| **3**  | 1 | 0 | 0 | 0 | 0 |
| **2**  | 0 | 2 | 0 | 0 | 0 |
| **1**  | 0 | 0 | 3 | 0 | 0 |

**Maximum Length**: 3

- Steps for DP table:
    - Define the state transition:
        -  if nums1[i - 1] != nums2[j - 1]:  dp[j] = prev + 1
        -  if nums1[i - 1] == nums2[j - 1]: dp[j] = 0 
    - Determine the order of traversal: the inner loop needs to be backward to avoid dupes
- 
```python
# solution 1: dp table
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        # 创建一个二维数组 dp，用于存储最长公共子数组的长度
        dp = [[0] * (len(nums2) + 1) for _ in range(len(nums1) + 1)]
        # 记录最长公共子数组的长度
        result = 0

        # 遍历数组 nums1
        for i in range(1, len(nums1) + 1):
            # 遍历数组 nums2
            for j in range(1, len(nums2) + 1):
                # 如果 nums1[i-1] 和 nums2[j-1] 相等
                if nums1[i - 1] == nums2[j - 1]:
                    # 在当前位置上的最长公共子数组长度为前一个位置上的长度加一
                    dp[i][j] = dp[i - 1][j - 1] + 1
                # 更新最长公共子数组的长度
                if dp[i][j] > result:
                    result = dp[i][j]

        # 返回最长公共子数组的长度
        return result

# solution 2: 1 d array 
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        # 创建一个一维数组 dp，用于存储最长公共子数组的长度
        dp = [0] * (len(nums2) + 1)
        # 记录最长公共子数组的长度
        result = 0

        # 遍历数组 nums1
        for i in range(1, len(nums1) + 1):
            # 用于保存上一个位置的值
            prev = 0
            # 遍历数组 nums2
            for j in range(1, len(nums2) + 1):
                # 保存当前位置的值，因为会在后面被更新
                current = dp[j]
                # 如果 nums1[i-1] 和 nums2[j-1] 相等
                if nums1[i - 1] == nums2[j - 1]:
                    # 在当前位置上的最长公共子数组长度为上一个位置的长度加一
                    dp[j] = prev + 1
                    # 更新最长公共子数组的长度
                    if dp[j] > result:
                        result = dp[j]
                else:
                    # 如果不相等，将当前位置的值置为零
                    dp[j] = 0
                # 更新 prev 变量为当前位置的值，供下一次迭代使用
                prev = current

        # 返回最长公共子数组的长度
        return result

```
Time: **O(n*m)**              
Space: **O(n*m)** for solution 1 and **O(m)** for solution 2

