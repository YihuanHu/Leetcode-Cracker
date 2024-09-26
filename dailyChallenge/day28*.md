# Day28 Greedy Algo Part2

## LC 122 best-time-to-buy-and-sell-stock-ii
[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/description/)   
[Cousrse Link](https://programmercarl.com/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII.html)    
- Local optimality: Collecting positive profits each day. Global optimality: Achieving the maximum profit
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        result = 0
        for i in range(1, len(prices)): # only collect postive prediff
            result += max(prices[i] - prices[i - 1], 0)
        return result
```
Time: **O(n)**     
Space: **O(1)** 

##  LC 55 jump-game
[Link](https://leetcode.com/problems/jump-game/)   
[Cousrse Link](https://programmercarl.com/0055.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8F.html)

- Local optimality: At each step, take the maximum jump distance (to achieve the largest coverage)
- Global optimality: Ultimately achieving the maximum coverage to see if it is possible to reach the endpoint.
```python
## for循环
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        cover = 0
        if len(nums) == 1: return True
        for i in range(len(nums)):
            if i <= cover:
                cover = max(i + nums[i], cover)
                if cover >= len(nums) - 1: return True
        return False
```
Time: **O(n)**     
Space: **O(1)** 


##  LC 45 jump-game-ii
[Link](https://leetcode.com/problems/jump-game-ii/description/)   
[Cousrse Link](https://programmercarl.com/0045.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8FII.html#%E6%80%9D%E8%B7%AF)    
- Increase the maximum coverage with the minimum number of steps until the coverage reaches the endpoint. Within this range, the minimum number of steps will definitely allow us to jump to the target, regardless of the specifics of the jumps.
- If the moving index reaches the maximum coverage distance for the current step and has not yet reached the endpoint, then an additional step must be taken to increase the coverage range until it encompasses the endpoint.
- Local optimality: At each step, update the next distance
- Global optimality: Ultimately achieving the maximum coverage to see if it is possible to reach the endpoint.
```python
class Solution:
    def jump(self, nums):
        if len(nums) == 1:
            return 0
        
        cur_distance = 0  # 当前覆盖最远距离下标
        ans = 0  # 记录走的最大步数
        next_distance = 0  # 下一步覆盖最远距离下标
        
        for i in range(len(nums)):
            next_distance = max(nums[i] + i, next_distance)  # 更新下一步覆盖最远距离下标
            if i == cur_distance:  # 遇到当前覆盖最远距离下标
                ans += 1  # 需要走下一步
                cur_distance = next_distance  # 更新当前覆盖最远距离下标（相当于加油了）
                if next_distance >= len(nums) - 1:  # 当前覆盖最远距离达到数组末尾，不用再做ans++操作，直接结束
                    break
        
        return ans
```
Time: **O(n)**     
Space: **O(1)** 

##  LC 1005 maximize-sum-of-array-after-k-negations
[Link](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/)   
[Cousrse Link](https://programmercarl.com/1005.K%E6%AC%A1%E5%8F%96%E5%8F%8D%E5%90%8E%E6%9C%80%E5%A4%A7%E5%8C%96%E7%9A%84%E6%95%B0%E7%BB%84%E5%92%8C.html)    
- Two greedy algo
    - For negative
        - Local optimality: Transform large absolute negative numbers into positive numbers, maximizing the current value
        - Global optimality: Maximize the total sum of the entire array
    - For positive
        - Local optimality: Only find the smallest positive integer to reverse, maximizing the current value and sum
        - Global optimality: Maximize the total sum of the entire array
        - for example, in the positive integer array {5, 3, 1}, reversing 1 results in -1, which is much larger than reversing 5 to get -5
- For code
- Why use  ``K % 2 == 1`` ? what if there are 2 left in k?
    - If k is odd, need to reverse the smallest positve number
    - If k is even, reverse it twice and keep it the same
```python
class Solution:
    def largestSumAfterKNegations(self, A: List[int], K: int) -> int:
        A.sort(key=lambda x: abs(x), reverse=True)  # 第一步：按照绝对值降序排序数组A

        for i in range(len(A)):  # 第二步：执行K次取反操作
            if A[i] < 0 and K > 0:
                A[i] *= -1
                K -= 1

        if K % 2 == 1:  # 第三步：如果K还有剩余奇数次数，将绝对值最小的元素取反 偶数次可以忽略 因为可以reverse twice
            A[-1] *= -1

        result = sum(A)  # 第四步：计算数组A的元素和
        return result
```
Time: **O(n*logn)**     
Space: **O(1)** 
