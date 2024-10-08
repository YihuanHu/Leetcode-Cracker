# Day36 Dynamic Programming Part5 Unbounded Bag Problem
## Knapsack/Bag Problem
- Unbounded Knapsack Problem: use an unlimited number of each item
- For ieteration of dp table/1D array, no matter the inner loop or outter loop order. But we need to iterate the bag weight from small to large to include select one item multiple times.
```python
# sample 1
def test_CompletePack():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bagWeight = 4
    dp = [0] * (bagWeight + 1)
    for i in range(len(weight)):  # 遍历物品
        for j in range(weight[i], bagWeight + 1):  # 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i])
    print(dp[bagWeight])

test_CompletePack()

# sample 2
def test_CompletePack():
    weight = [1, 3, 4]
    value = [15, 20, 30]
    bagWeight = 4

    dp = [0] * (bagWeight + 1)

    for j in range(bagWeight + 1):  # 遍历背包容量
        for i in range(len(weight)):  # 遍历物品
            if j - weight[i] >= 0:
                dp[j] = max(dp[j], dp[j - weight[i]] + value[i])

    print(dp[bagWeight])

test_CompletePack()
```

##  LC 518 coin-change-ii
[Link](https://leetcode.com/problems/coin-change-ii/description/)   
[Cousrse Link](https://programmercarl.com/0518.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2II.html)
- **combinations: the outer for loop should iterate over the items, and the inner for loop should iterate over the knapsack capacities**
- **permutations: the outer for loop should iterate over the knapsack capacities, and the inner for loop should iterate over the items**
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:number of COMBINATIONS of getting exact amount j
    - Define the state transition deleting i:
    -   To find the number of ways to fill the knapsack, the formula is: dp[j] += dp[j - nums[i]]
    - How to initialize the DP array: 
        -  dp[0] = 1
    - Determine the order of traversal: **item is the outer loop and weight is the inner**
      - This way we only iterate each item once {1,5}
      - If reverse, we gonna get both {1,5} and {5,1}

    - Provide an example to derive the DP array:
        -  amount = 5, coins = [1, 2, 5]
        - dp = [[1,1,1,1,1,1], [1,1,2,2,3,3] , [1,1,2,2,3,4]]
```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [0]*(amount + 1)
        dp[0] = 1
        # 遍历物品
        for i in range(len(coins)):
            # 遍历背包
            for j in range(coins[i], amount + 1):
                dp[j] += dp[j - coins[i]]
        return dp[amount]
```
Time: **O(n^m)** m is amount and n is coin list length               
Space: **O(m)** 

##  LC 377 combination-sum-iv
[Link](https://leetcode.com/problems/combination-sum-iv/description/)   
[Cousrse Link](https://programmercarl.com/0377.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C%E2%85%A3.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- Compared with the above LC 518
- Steps for DP:
    - Define the dp[j]:
        - dp[j]:number of PERMUTATIONS of getting exact amount j
    - Define the state transition deleting i:
    -   To find the number of ways to fill the knapsack, the formula is: dp[j] += dp[j - nums[i]]
    - How to initialize the DP array: 
        -  dp[0] = 1 otherewise the calculation is meaningless
    - Determine the order of traversal: **weight is the outer loop and item is the inner**
      - Same above
    - Provide an example to derive the DP array:
        -  target = 4, coins = [1, 2, 3]     
        -[  [1, 0, 0, 0, 0],  # Initial state (dp[0] = 1)    
              [1, 1, 0, 0, 0],  # After calculating dp[1] = dp[0]     
              [1, 1, 2, 0, 0],  # After calculating dp[2] = dp[1] + dp[0]     
              [1, 1, 2, 4, 0],  # After calculating dp[3] = dp[2] + dp[1] + dp[0]     
              [1, 1, 2, 4, 7]   # After calculating dp[4] = dp[3] + dp[2] + dp[1]     
            ]      

```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0] * (target + 1)
        dp[0] = 1
        for i in range(1, target + 1):  # 遍历背包
            for j in range(len(nums)):  # 遍历物品
                if i - nums[j] >= 0:
                    dp[i] += dp[i - nums[j]]
        return dp[target]


```
Time: **O(target * n)**  n is nums list length               
Space: **O(target)** 

##  Advanced Climbing Ladder
[Cousrse Link]https://programmercarl.com/0070.%E7%88%AC%E6%A5%BC%E6%A2%AF%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E7%89%88%E6%9C%AC.html)
- One example of Permutation
- Compared with LC 57
