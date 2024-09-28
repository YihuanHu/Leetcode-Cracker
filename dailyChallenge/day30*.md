# Day30 Greedy Algo Part4

## LC 452 minimum-number-of-arrows-to-burst-balloons
[Link](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)   
[Cousrse Link](https://programmercarl.com/0452.%E7%94%A8%E6%9C%80%E5%B0%91%E6%95%B0%E9%87%8F%E7%9A%84%E7%AE%AD%E5%BC%95%E7%88%86%E6%B0%94%E7%90%83.html)    
- Local optimality: When balloons overlap, shoot them together to minimize the number of arrows used
- Global optimality: Find the starting position that allows completing a full circle.
```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if len(points) == 0: return 0
        points.sort(key=lambda x: x[0])
        result = 1
        for i in range(1, len(points)):
            if points[i][0] > points[i - 1][1]: # 气球i和气球i-1不挨着，注意这里不是>=
                result += 1     
            else:
                points[i][1] = min(points[i - 1][1], points[i][1]) # 更新重叠气球最小右边界 by pick the min of i & i-1 for common
        return result
```
Time: **O(n*Logn)**     
Space: **O(1)** 

##  LC 435 non-overlapping-intervals
[Link](https://leetcode.com/problems/non-overlapping-intervals/description/)   
[Cousrse Link](https://programmercarl.com/0435.%E6%97%A0%E9%87%8D%E5%8F%A0%E5%8C%BA%E9%97%B4.html)
- Pretty like 452 to count the nonn-overlapping 
```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0
        
        intervals.sort(key=lambda x: x[0])  # 按照左边界升序排序
        
        result = 1  # 不重叠区间数量，初始化为1，因为至少有一个不重叠的区间
        
        for i in range(1, len(intervals)):
            if intervals[i][0] >= intervals[i - 1][1]:  # 没有重叠
                result += 1
            else:  # 重叠情况
                intervals[i][1] = min(intervals[i - 1][1], intervals[i][1])  # 更新重叠区间的右边界
        
        return len(intervals) - result
```
Time: **O(n)**     
Space: **O(1)** 


##  LC 763 partition-labels
[Link](https://leetcode.com/problems/partition-labels/description/)   
[Cousrse Link](https://programmercarl.com/0763.%E5%88%92%E5%88%86%E5%AD%97%E6%AF%8D%E5%8C%BA%E9%97%B4.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)      
- for solution 2, update the the recent last_occurance since any smaller last_occurance gonna be included to avoid one char in several partition
```python
# solution #1 use similar overlapping structure 
class Solution:
    def countLabels(self, s):
        # 初始化一个长度为26的区间列表，初始值为负无穷
        hash = [[float('-inf'), float('-inf')] for _ in range(26)]
        hash_filter = []
        for i in range(len(s)):
            if hash[ord(s[i]) - ord('a')][0] == float('-inf'):
                hash[ord(s[i]) - ord('a')][0] = i
            hash[ord(s[i]) - ord('a')][1] = i
        for i in range(len(hash)):
            if hash[i][0] != float('-inf'):
                hash_filter.append(hash[i])
        return hash_filter

    def partitionLabels(self, s):
        res = []
        hash = self.countLabels(s)
        hash.sort(key=lambda x: x[0])  # 按左边界从小到大排序
        rightBoard = hash[0][1]  # 记录最大右边界
        leftBoard = 0
        for i in range(1, len(hash)):
            if hash[i][0] > rightBoard:  # 出现分割点
                res.append(rightBoard - leftBoard + 1)
                leftBoard = hash[i][0]
            rightBoard = max(rightBoard, hash[i][1])
        res.append(rightBoard - leftBoard + 1)  # 最右端
        return res



# solution 2: use last occurance
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        last_occurrence = {}  # 存储每个字符最后出现的位置
        for i, ch in enumerate(s):
            last_occurrence[ch] = i

        result = []
        start = 0
        end = 0
        for i, ch in enumerate(s):
            end = max(end, last_occurrence[ch])  # 找到当前字符出现的最远位置
            if i == end:  # 如果当前位置是最远位置，表示可以分割出一个区间
                result.append(end - start + 1)
                start = i + 1

        return result
```
Time: **O(n*Logn)**     
Space: **O(n)** 
