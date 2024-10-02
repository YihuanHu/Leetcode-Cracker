# Day34 Dynamic Programming Part3 Knapsack/Bag Problem
## Knapsack/Bag Problem
- In general, Knapsack Problem is Given a set of items, each with a weight and a value, determine the maximum value that can be carried in a knapsack of a fixed capacity
- 0/1 Knapsack Problem:  Each item can either be included (1) or excluded (0).
- Unbounded Knapsack Problem: use an unlimited number of each item
- Bounded Knapsack Problem: generalization of the 0/1 knapsack problem where each item has a limit on the number of times

## 0/1 Knapsack Problem
[Cousrse Link](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html#%E6%80%9D%E8%B7%AF)    
- Steps for DP of DP table:
    - Define the dp[i][j]:
        - i for item
        - j for the knapsack weight
        - dp[i][j]:the maximum total value that can be obtained by taking **any items from indices [0-i] and placing them in a knapsack with a capacity of j**
    - Define the state transition: **dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i])**
        - Only two ways for item i:
        - Not Put in i since j < weight[i]: **dp[i - 1][j]**
        - Put in i since j >= weight[i]: **dp[i - 1][j - weight[i]] + value[i]** represents the maximum value putting item i and plus value of item i
    - How to initialize the DP array: initial first row * col
        - first row is item 0 value only if j > weight[0] : for j in range(weight[0], bagweight + 1): dp[0][j] = value[0]
        - first col all 0 since bag limit is 0: can be ignored since we initiate whole dp as 0
    - Determine the order of traversal: two nested loop for item and bag weight from small to large since dp[i][j] depends on dp[i - 1][j] and dp[i - 1][j - weight[i]] which are on the left upper side. Outer loop can be item or bag weight. Both are fine.
    - Provide an example to derive the DP array:
        - bag weight = 4,
        - item weight = [1,3,4]
        - item value = [15,20,30]
        - dp = [ [0,15,15,15,15], [0,15,15,20,35], [0,15,15,20,35] ]


-----------
- Steps for DP of rolling array:
    - Define the dp[j]:
        - dp[j]:the maximum total value that can be obtained by taking **any items and placing them in a knapsack with a capacity of j**
    - Define the state transition deleting i: **dp[j] = max(dp[j], dp[j - weight[i]] + value[i])**
    - How to initialize the DP array: 
        -  we initiate whole dp as 0
    - Determine the order of traversal: **reverse loop for bag and item first then bag**
        -  The inener bag loop from large to small:
            -  dp[2] = dp[2 - weight[0]] + value[0] = 15 （dp数组已经都初始化为0）
            -  dp[1] = dp[1 - weight[0]] + value[0] = 15
        -  Only can iterate item first and then bag otherwise each j can only put one item
        -  iterate bag then item: If you loop over j (the capacity) in the outer loop and then iterate over the items i in the inner loop, you are updating dp[j] for the current capacity before considering all items
    - Provide an example to derive the DP array:
        - bag weight = 4,
        - item weight = [1,3,4]
        - item value = [15,20,30]
        - dp = [ [0,15,15,15,15], [0,15,15,20,35], [0,15,15,20,35] ]


```python
# solution 1: dp table
n, bagweight = map(int, input().split())

weight = list(map(int, input().split()))
value = list(map(int, input().split()))

dp = [[0] * (bagweight + 1) for _ in range(n)]

for j in range(weight[0], bagweight + 1):
    dp[0][j] = value[0]

for i in range(1, n):
    for j in range(bagweight + 1):
        if j < weight[i]:
            dp[i][j] = dp[i - 1][j]
        else:
            dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i])

print(dp[n - 1][bagweight])

# solution 2: rolling array
n, bagweight = map(int, input().split())
weight = list(map(int, input().split()))
value = list(map(int, input().split()))

dp = [0] * (bagweight + 1)  # 创建一个动态规划数组dp，初始值为0

dp[0] = 0  # 初始化dp[0] = 0,背包容量为0，价值最大为0

for i in range(n):  # 应该先遍历物品，如果遍历背包容量放在上一层，那么每个dp[j]就只会放入一个物品
    for j in range(bagweight, weight[i]-1, -1):  # 倒序遍历背包容量是为了保证物品i只被放入一次
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

print(dp[bagweight])

```
Time: **O(m*n)**     
Space: **O(m*n)** for solution 1 and **O(n)** for solution 2


##  LC 416 partition-equal-subset-sum
[Link](https://leetcode.com/problems/partition-equal-subset-sum/description/)   
[Cousrse Link](https://programmercarl.com/0416.%E5%88%86%E5%89%B2%E7%AD%89%E5%92%8C%E5%AD%90%E9%9B%86.html)
- Typical 0/1 bag where value = weight = numbers and we need to find out if bag weight = sum / 2
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:the maximum total value that can be obtained by taking **any items and placing them in a knapsack with a capacity of j**
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


