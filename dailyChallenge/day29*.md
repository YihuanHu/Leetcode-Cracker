# Day29 Greedy Algo Part3

## LC 134 gas-station
[Link](https://leetcode.com/problems/gas-station/description/)   
[Cousrse Link](https://programmercarl.com/0134.%E5%8A%A0%E6%B2%B9%E7%AB%99.html)    
- Local optimality: Once the cumulative sum curSum of rest[i] becomes less than 0, the starting position must be at least i + 1, because starting from any position before i will not work.
- Global optimality: Find the starting position that allows completing a full circle.
```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        curSum = 0  # 当前累计的剩余油量
        totalSum = 0  # 总剩余油量
        start = 0  # 起始位置
        
        for i in range(len(gas)):
            curSum += gas[i] - cost[i]
            totalSum += gas[i] - cost[i]
            
            if curSum < 0:  # 当前累计剩余油量curSum小于0
                start = i + 1  # 起始位置更新为i+1
                curSum = 0  # curSum重新从0开始累计
        
        if totalSum < 0:
            return -1  # 总剩余油量totalSum小于0，说明无法环绕一圈
        return start
```
Time: **O(n)**     
Space: **O(1)** 

##  LC 135 candy
[Link](https://leetcode.com/problems/candy/description/)   
[Cousrse Link](https://programmercarl.com/0135.%E5%88%86%E5%8F%91%E7%B3%96%E6%9E%9C.html)
- Two greedy algo:
    - One pass is from left to right, comparing where the score of the left < right 
    - Another pass is from right to left, comparing where the score of the right < left
- Local optimality: Take the maximum between candyVec[i + 1] + 1 and candyVec[i] to ensure that the number of candies for the i-th child is greater than both the left and right children
- Global optimality: Among adjacent children, the child with the higher score receives more candies
```python
## for循环
class Solution:
    def candy(self, ratings: List[int]) -> int:
        candyVec = [1] * len(ratings)
        
        # 从前向后遍历，处理右侧比左侧评分高的情况
        for i in range(1, len(ratings)):
            if ratings[i] > ratings[i - 1]:
                candyVec[i] = candyVec[i - 1] + 1
        
        # 从后向前遍历，处理左侧比右侧评分高的情况
        for i in range(len(ratings) - 2, -1, -1):
            if ratings[i] > ratings[i + 1]:
                candyVec[i] = max(candyVec[i], candyVec[i + 1] + 1)
        
        # 统计结果
        result = sum(candyVec)
        return result
```
Time: **O(n)**     
Space: **O(n)** 


##  LC 860 lemonade-change
[Link](https://leetcode.com/problems/lemonade-change/description/)   
[Cousrse Link](https://programmercarl.com/0860.%E6%9F%A0%E6%AA%AC%E6%B0%B4%E6%89%BE%E9%9B%B6.html)      
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

##  LC 860 lemonade-change
[Link](https://leetcode.com/problems/lemonade-change/description/)   
[Cousrse Link](https://programmercarl.com/0860.%E6%9F%A0%E6%AA%AC%E6%B0%B4%E6%89%BE%E9%9B%B6.html)    
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
