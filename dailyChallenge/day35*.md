# Day35 Dynamic Programming Part4

## LC 1049 last-stone-weight-ii
[Link](https://leetcode.com/problems/last-stone-weight-ii/description/)   
[Cousrse Link](https://programmercarl.com/1049.%E6%9C%80%E5%90%8E%E4%B8%80%E5%9D%97%E7%9F%B3%E5%A4%B4%E7%9A%84%E9%87%8D%E9%87%8FII.html)    
- like a 0/1 bag problem where we trying to find out two divided subset which near sum / 2 e.g LC 416 partition-equal-subset-sum
- we get target = sum / 2 by floor function so sum - dp[target] > dp[target] and we return this diff as the remaining stone
- Steps for DP:
    - Define the dp[j]:
        - a knapsack with a capacity of j, the maximum weight that can be carried is represented by dp[j]
    - Define the state transition deleting i: **dp[j] = max(dp[j], dp[j - stones[i]] + stones[i])**
    - How to initialize the DP array: 
        -  we initiate whole dp as 0 since all are postive integer
        -  if we have negative, we should iniate it as negative infinite
    - Determine the order of traversal: **reverse loop for bag and item first then bag** see Day 34 LC 416 
    - Provide an example to derive the DP array:
        - stones = [2,4,1,1] => bag weight = (2+4+1+1)/2 = 4 
        - dp = [0,1,2,3,4]
```python
# rolling array/1 D array
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        dp = [0] * 15001
        total_sum = sum(stones)
        target = total_sum // 2

        for stone in stones:  # 遍历物品
            for j in range(target, stone - 1, -1):  # 遍历背包
                dp[j] = max(dp[j], dp[j - stone] + stone)

        return total_sum - dp[target] - dp[target]

```
Time: **O(m*n)**     
Space: **O(n)** 


##  LC 416 partition-equal-subset-sum
[Link](https://leetcode.com/problems/partition-equal-subset-sum/description/)   
[Cousrse Link](https://programmercarl.com/0416.%E5%88%86%E5%89%B2%E7%AD%89%E5%92%8C%E5%AD%90%E9%9B%86.html)
- Typical 0/1 bag where value = weight = numbers and we need to find out if bag weight = sum / 2
- Steps for DP:
    - Define the dp[j]:
        - a knapsack with a capacity of j, the maximum weight that can be carried is represented by dp[j]
    - Define the state transition deleting i: **dp[j] = max(dp[j], dp[j - nums[i]] + nums[i])**
    - How to initialize the DP array: 
        -  we initiate whole dp as 0 since all are postive integer
        -  if we have negative, we should iniate it as negative infinite
    - Determine the order of traversal: **reverse loop for bag and item first then bag**
        -  The inener bag loop from large to small:
            -  dp[2] = dp[2 - weight[0]] + value[0] = 15 （dp数组已经都初始化为0）
            -  dp[1] = dp[1 - weight[0]] + value[0] = 15
        -  Only can iterate item first and then bag otherwise each j can only put one item
        -  iterate bag then item: If you loop over j (the capacity) in the outer loop and then iterate over the items i in the inner loop, you are updating dp[j] for the current capacity before considering all items
    - Provide an example to derive the DP array:
        - nums = [1,5,11,5] => bag weight = (1+5+11+5)/2 = 11
        - dp = [0,1,1,1,15,6,6,6,6,10,11]
```python
# solution 1: dp table
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        _sum = 0

        # dp[i]中的i表示背包内总和
        # 题目中说：每个数组中的元素不会超过 100，数组的大小不会超过 200
        # 总和不会大于20000，背包最大只需要其中一半，所以10001大小就可以了
        dp = [0] * 10001
        for num in nums:
            _sum += num
        # 也可以使用内置函数一步求和
        # _sum = sum(nums)
        if _sum % 2 == 1:
            return False
        target = _sum // 2

        # 开始 0-1背包
        for num in nums:
            for j in range(target, num - 1, -1):  # 每一个元素一定是不可重复放入，所以从大到小遍历
                dp[j] = max(dp[j], dp[j - num] + num)

        # 集合中的元素正好可以凑成总和target
        if dp[target] == target:
            return True
        return False

# solution 2: use rolling arrays / 1D array
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        if sum(nums) % 2 != 0:
            return False
        target = sum(nums) // 2
        dp = [0] * (target + 1)
        for num in nums:
            for j in range(target, num-1, -1):
                dp[j] = max(dp[j], dp[j-num] + num)
        return dp[-1] == target

```
Time: **O(n^2)**      
Space: **O(n^2)** for solution 1 and **O(n)** for solution 2 since we can treat the fixed 

-  we can use T/F to replace the exact numbers
```python
# solution 1: dp table
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        
        total_sum = sum(nums)

        if total_sum % 2 != 0:
            return False

        target_sum = total_sum // 2
        dp = [[False] * (target_sum + 1) for _ in range(len(nums) + 1)]

        # 初始化第一行（空子集可以得到和为0）
        for i in range(len(nums) + 1):
            dp[i][0] = True

        for i in range(1, len(nums) + 1):
            for j in range(1, target_sum + 1):
                if j < nums[i - 1]:
                    # 当前数字大于目标和时，无法使用该数字
                    dp[i][j] = dp[i - 1][j]
                else:
                    # 当前数字小于等于目标和时，可以选择使用或不使用该数字
                    dp[i][j] = dp[i - 1][j] or dp[i - 1][j - nums[i - 1]]

        return dp[len(nums)][target_sum]

# solution 2: use rolling arrays / 1D array
class Solution:
    def canPartition(self, nums: List[int]) -> bool:

        total_sum = sum(nums)

        if total_sum % 2 != 0:
            return False

        target_sum = total_sum // 2
        dp = [False] * (target_sum + 1)
        dp[0] = True

        for num in nums:
            # 从target_sum逆序迭代到num，步长为-1
            for i in range(target_sum, num - 1, -1):
                dp[i] = dp[i] or dp[i - num]
        return dp[target_sum]

```



