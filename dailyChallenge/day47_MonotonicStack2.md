# Day47 Monotonic Stack #2

##  LC 42 trapping-rain-water
[Link](https://leetcode.com/problems/trapping-rain-water/description/)   
[Cousrse Link](https://programmercarl.com/0042.%E6%8E%A5%E9%9B%A8%E6%B0%B4.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- by col:
  - use two pointer like solution 1
  - h = min(lHeight, rHeight) - height[i]
  - maxLeft[i] = max(height[i], maxLeft[i - 1])
  - maxRight[i] = max(height[i], maxRight[i + 1])
- by row:
  - use increasing monotonic stack like solution 2
  - right is i, mid is top, left is top of top
  - h = min(height[st.top()], height[i]) - height[mid]
  - w = i - st.top() - 1 

```python
# solution 1
class Solution:
    def trap(self, height: List[int]) -> int:
        leftheight, rightheight = [0]*len(height), [0]*len(height)

        leftheight[0]=height[0]
        for i in range(1,len(height)):
            leftheight[i]=max(leftheight[i-1],height[i])
        rightheight[-1]=height[-1]
        for i in range(len(height)-2,-1,-1):
            rightheight[i]=max(rightheight[i+1],height[i])

        result = 0
        for i in range(0,len(height)):
            summ = min(leftheight[i],rightheight[i])-height[i]
            result += summ
        return result

# solution 2
class Solution:
    def trap(self, height: List[int]) -> int:
        # 单调栈
        '''
        单调栈是按照 行 的方向来计算雨水
        从栈顶到栈底的顺序：从小到大
        通过三个元素来接水：栈顶，栈顶的下一个元素，以及即将入栈的元素
        雨水高度是 min(凹槽左边高度, 凹槽右边高度) - 凹槽底部高度
        雨水的宽度是 凹槽右边的下标 - 凹槽左边的下标 - 1（因为只求中间宽度）
        '''
        # stack储存index，用于计算对应的柱子高度
        stack = [0]
        result = 0
        for i in range(1, len(height)):
            # 情况一
            if height[i] < height[stack[-1]]:
                stack.append(i)

            # 情况二
            # 当当前柱子高度和栈顶一致时，左边的一个是不可能存放雨水的，所以保留右侧新柱子
            # 需要使用最右边的柱子来计算宽度
            elif height[i] == height[stack[-1]]:
                stack.pop()
                stack.append(i)

            # 情况三
            else:
                # 抛出所有较低的柱子
                while stack and height[i] > height[stack[-1]]:
                    # 栈顶就是中间的柱子：储水槽，就是凹槽的地步
                    mid_height = height[stack[-1]]
                    stack.pop()
                    if stack:
                        right_height = height[i]
                        left_height = height[stack[-1]]
                        # 两侧的较矮一方的高度 - 凹槽底部高度
                        h = min(right_height, left_height) - mid_height
                        # 凹槽右侧下标 - 凹槽左侧下标 - 1: 只求中间宽度
                        w = i - stack[-1] - 1
                        # 体积：高乘宽
                        result += h * w
                stack.append(i)
        return result

```
Time: **O(n)**                                  
Space: **O(n)** 

##  LC 84 largest-rectangle-in-histogram
[Link](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)   
[Cousrse Link](https://programmercarl.com/0084.%E6%9F%B1%E7%8A%B6%E5%9B%BE%E4%B8%AD%E6%9C%80%E5%A4%A7%E7%9A%84%E7%9F%A9%E5%BD%A2.html)
- Compared with the above LC 42. Similar logic but there is 
- be careful on the indexing 
- Use increaseing stack (from top to bottom)
  -  If T[i] < T[st.top()], add to stack
  -  If T[i] = T[st.top()], add to stack
  -  If T[i] > T[st.top()], pop and calculate res till elements inside is larger than top

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        result = [-1]*len(nums1)
        stack = [0]
        for i in range(1,len(nums2)):
            # 情况一情况二
            if nums2[i]<=nums2[stack[-1]]:
                stack.append(i)
            # 情况三
            else:
                while len(stack)!=0 and nums2[i]>nums2[stack[-1]]:
                    if nums2[stack[-1]] in nums1: #check top exist in nums1
                        index = nums1.index(nums2[stack[-1]])
                        result[index]=nums2[i]
                    stack.pop()                 
                stack.append(i)
        return result
```
Time: **O(n)**                   
Space: **O(n)** 

##  LC 503 next-greater-element-ii
[Link](https://leetcode.com/problems/next-greater-element-ii/description/)   
[Cousrse Link](https://programmercarl.com/0496.%E4%B8%8B%E4%B8%80%E4%B8%AA%E6%9B%B4%E5%A4%A7%E5%85%83%E7%B4%A0I.html)
- Similar to LC 739 but it a circular array so we can iterate it twice
```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        dp = [-1] * len(nums)
        stack = []
        for i in range(len(nums)*2):
            while(len(stack) != 0 and nums[i%len(nums)] > nums[stack[-1]]):
                    dp[stack[-1]] = nums[i%len(nums)]
                    stack.pop()
            stack.append(i%len(nums))
        return dp
```
Time: **O(n)**                   
Space: **O(n)** 
