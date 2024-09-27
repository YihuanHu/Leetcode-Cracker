# Day30 Greedy Algo Part4

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
- Focus on handling one side before addressing the other; don't try to balance both sides at the same time.
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
- Local optimality: When encountering a bill of 20, prioritize using a 10-dollar bill to make the change for this transaction
- Global optimality: Successfully complete the change for all bills
```python
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        five = 0
        ten = 0
        twenty = 0
        
        for bill in bills:
            # 情况一：收到5美元
            if bill == 5:
                five += 1
            
            # 情况二：收到10美元
            if bill == 10:
                if five <= 0:
                    return False
                ten += 1
                five -= 1
            
            # 情况三：收到20美元
            if bill == 20:
                # 先尝试使用10美元和5美元找零
                if five > 0 and ten > 0:
                    five -= 1
                    ten -= 1
                    #twenty += 1
                # 如果无法使用10美元找零，则尝试使用三张5美元找零
                elif five >= 3:
                    five -= 3
                    #twenty += 1
                else:
                    return False
        
        return True
```
Time: **O(n)**     
Space: **O(1)** 

##  LC 406 queue-reconstruction-by-height
[Link](https://leetcode.com/problems/queue-reconstruction-by-height/description/)   
[Cousrse Link](https://programmercarl.com/0406.%E6%A0%B9%E6%8D%AE%E8%BA%AB%E9%AB%98%E9%87%8D%E5%BB%BA%E9%98%9F%E5%88%97.html)    
- Focus on handling one side before addressing the other; don't try to balance both sides at the same time
- **Two greedy algo**:
    - One pass is from left to right, comparing where the weight is decreasing
    - Another pass is from left to right of sorted people, insert at ki
- Local optimality: Prioritize inserting people based on the height of the tallest individuals, ensuring that the queue properties are satisfied after each insertion
- Global optimality: After completing all insertion operations, the entire queue satisfies the problem's queue properties
- The insert is based at index k
```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
    	# 先按照h维度的身高顺序从高到低排序。确定第一个维度
        # lambda返回的是一个元组：当-x[0](维度h）相同时，再根据x[1]（维度k）从小到大排序
        people.sort(key=lambda x: (-x[0], x[1]))
        que = []
	
	# 根据每个元素的第二个维度k，贪心算法，进行插入
        # people已经排序过了：同一高度时k值小的排前面。
        for p in people:
            que.insert(p[1], p)
        return que
```
Time: **O(n*logn + n^2)**     
Space: **O(n)** 
