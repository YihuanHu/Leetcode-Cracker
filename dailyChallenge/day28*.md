# Day28 Greedy Algo Part2
## Greedy
- The essence of greedy algorithms is to choose the local optimal solution at each stage to achieve a global optimal solution.
- Greedy on't follow a specific pattern; fundamentally, they rely on common sense reasoning and counterexamples
- two ways of proving:
    - Proof by Contradiction
    - Mathematical Induction

## LC * 455 assign-cookies
[Link](https://leetcode.com/problems/assign-cookies/description/)   
[Cousrse Link](https://programmercarl.com/0455.%E5%88%86%E5%8F%91%E9%A5%BC%E5%B9%B2.html)    
- Use index to save one for loop
- Two logics
    - Use greed as loop from large to small: large cookis can satisfy needs large greed
        - What if loop greed from small to large: if there is one extrem greek, if should be fine
    - Use cookies as loop from small to large: small greed can be satisfied by small cookies
        - What if loop cookies from large to small: if there is one extrem large greed, then no one can eat
- What to do 
```python
# solution 1
class Solution:
    def findContentChildren(self, g, s):
        g.sort()  # 将孩子的贪心因子排序
        s.sort()  # 将饼干的尺寸排序
        index = len(s) - 1  # 饼干数组的下标，从最后一个饼干开始
        result = 0  # 满足孩子的数量
        for i in range(len(g)-1, -1, -1):  # 遍历胃口，从最后一个孩子开始
            if index >= 0 and s[index] >= g[i]:  # 遍历饼干
                result += 1
                index -= 1
        return result

# solution 2
class Solution:
    def findContentChildren(self, g, s):
        g.sort()  # 将孩子的贪心因子排序
        s.sort()  # 将饼干的尺寸排序
        index = 0
        for i in range(len(s)):  # 遍历饼干
            if index < len(g) and g[index] <= s[i]:  # 如果当前孩子的贪心因子小于等于当前饼干尺寸
                index += 1  # 满足一个孩子，指向下一个孩子
        return index  # 返回满足的孩子数目
```
Time: **O(n * logn)**     
Space: **O(1)** 

##  LC 376 wiggle-subsequence
[Link](https://leetcode.com/problems/wiggle-subsequence/description/)   
[Cousrse Link](https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html)
  
- Consider 3 cases mainly for flat:
    - Situation 1: There is a flat section in an uphill and downhill slope.
    - Situation 2: The two ends of the array.
    - Situation 3: There is a flat section in a monotonic slope.
- can also use dp 
```python
class Solution:
    def wiggleMaxLength(self, nums):
        if len(nums) <= 1:
            return len(nums)  # 如果数组长度为0或1，则返回数组长度
        curDiff = 0  # 当前一对元素的差值
        preDiff = 0  # 前一对元素的差值
        result = 1  # 记录峰值的个数，初始为1（默认最右边的元素被视为峰值）
        for i in range(len(nums) - 1):
            curDiff = nums[i + 1] - nums[i]  # 计算下一个元素与当前元素的差值
            # 如果遇到一个峰值
            if (preDiff <= 0 and curDiff > 0) or (preDiff >= 0 and curDiff < 0):
                result += 1  # 峰值个数加1
                preDiff = curDiff  # 注意这里，只在摆动变化的时候更新preDiff
        return result  # 返回最长摆动子序列的长度
```
Time: **O(n)**     
Space: **O(1)** 


##  LC 53 maximum-subarray
[Link](https://leetcode.com/problems/maximum-subarray/description/)   
[Cousrse Link](https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.html)    
- Every time the current sum(count) < 0 , reset the count since add negative sum will for sure decrease the later sum
```python
class Solution:
    def maxSubArray(self, nums):
        result = float('-inf')  # 初始化结果为负无穷大
        count = 0
        for i in range(len(nums)):
            count += nums[i]
            if count > result:  # 取区间累计的最大值（相当于不断确定最大子序终止位置）
                result = count
            if count <= 0:  # 相当于重置最大子序起始位置，因为遇到负数一定是拉低总和
                count = 0
        return result

```
Time: **O(n)**     
Space: **O(1)** 


