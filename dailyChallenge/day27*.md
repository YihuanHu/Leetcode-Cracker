# Day27 Greedy Algo Part1
## Greedy
- The essence of greedy algorithms is to choose the local optimal solution at each stage to achieve a global optimal solution.
- Greedy on't follow a specific pattern; fundamentally, they rely on common sense reasoning and counterexamples
- two ways of proving:
    - Proof by Contradiction
    - Mathematical Induction

## LC 455 assign-cookies
[Link](https://leetcode.com/problems/assign-cookies/description/)   
[Cousrse Link](https://programmercarl.com/0455.%E5%88%86%E5%8F%91%E9%A5%BC%E5%B9%B2.html)    
- Use index to save one for loop
- Two logics
    - Use greed as loop from large to small: large cookis can satisfy needs large greed
    - Use cookies as loop from small to large: small greed can be satisfied by small cookies
        - What if loop small cookies from small to large
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


##  LC 47 permutations-ii
[Link](https://leetcode.cn/problems/permutations-ii/)   
[Cousrse Link](https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html)    
- **combination and permutation problems collect results at the leaf nodes of a tree structure, while the subset problem gathers results from all nodes on the tree.**
- Both used[i - 1] == true/false can be used
```python
class Solution:
    def permuteUnique(self, nums):
        nums.sort()  # 排序
        result = []
        self.backtracking(nums, [], [False] * len(nums), result)
        return result

    def backtracking(self, nums, path, used, result):
        if len(path) == len(nums):
            result.append(path[:])
            return
        for i in range(len(nums)):
            // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过 more effecient
            // 如果同一树层nums[i - 1]使用过则直接跳过
            if (i > 0 and nums[i] == nums[i - 1] and not used[i - 1]) or used[i]:  # de-dupe
                continue
            used[i] = True
            path.append(nums[i])
            self.backtracking(nums, path, used, result)
            path.pop()
            used[i] = False

```
Time: **O(n! * n )**     
Space: **O(n)** 

## Adds on
- [ ] 332, 51 , 37 for harder backtracking problems
- summary [Link](https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E6%80%BB%E7%BB%93.html#%E7%BB%84%E5%90%88%E9%97%AE%E9%A2%98)
    - the power of backtracking: using recursion to control the number of nested for loops

