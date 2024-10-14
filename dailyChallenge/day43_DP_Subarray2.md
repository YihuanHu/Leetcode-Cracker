# Day43 Dynamic Programming Part11 Subarray#2

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

# solution 2: greedy algo (Intuatively, for every non increasing, we gonna replace it with appropriate position)
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
Time: **O(n^2)** for solution 1 and **O(n*logn)** for solution 2                            
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

##  LC 718 maximum-length-of-repeated-subarray
[Link](https://leetcode.com/problems/maximum-length-of-repeated-subarray/description/)   
[Cousrse Link](https://programmercarl.com/0718.%E6%9C%80%E9%95%BF%E9%87%8D%E5%A4%8D%E5%AD%90%E6%95%B0%E7%BB%84.html)
- Similar to LC 122 but with a transaction fee
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

