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
        - dp[j]:number of combinations of getting exact amount j
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



