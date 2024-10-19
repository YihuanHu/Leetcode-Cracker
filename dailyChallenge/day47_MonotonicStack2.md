# Day47 Monotonic Stack #2

##  LC 42 trapping-rain-water
[Link](https://leetcode.com/problems/trapping-rain-water/description/)   
[Cousrse Link](https://programmercarl.com/0042.%E6%8E%A5%E9%9B%A8%E6%B0%B4.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- we are looking a **凹** concave
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
- looking for a **凸** convex
- Compared with the above LC 42. Similar logic but this is looking for the first decreasing values instead of increasing
- we need to add 0 at start and end since we can use it
- by col:
  - use two pointer like solution 1 and we are looking for **index**
  - rectangle = heights[i] * (min_right_index[i] - min_left_index[i] - 1)
  - while temp >= 0 and heights[temp] >= heights[i]: temp = min_left_index[temp]
  - while temp < size and heights[temp] >= heights[i]: temp = min_right_index[temp]
- by row:
  - use decreasing monotonic stack like solution 2
  - right is i, mid is top, left is top of top
  - width = right_index - left_index - 1
  - height = heights[mid_index]
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        # 从左向右遍历：以每一根柱子为主心骨（当前轮最高的参照物），迭代直到找到左侧和右侧各第一个矮一级的柱子
        res = 0

        for i in range(len(heights)):
            left = i
            right = i
            # 向左侧遍历：寻找第一个矮一级的柱子
            for _ in range(left, -1, -1):
                if heights[left] < heights[i]:
                    break
                left -= 1
            # 向右侧遍历：寻找第一个矮一级的柱子
            for _ in range(right, len(heights)):
                if heights[right] < heights[i]:
                    break
                right += 1
                
            width = right - left - 1
            height = heights[i]
            res = max(res, width * height)

        return res

# solution 1 双指针 
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        size = len(heights)
        # 两个DP数列储存的均是下标index
        min_left_index = [0] * size
        min_right_index = [0] * size
        result = 0

        # 记录每个柱子的左侧第一个矮一级的柱子的下标
        min_left_index[0] = -1  # 初始化防止while死循环
        for i in range(1, size):
            # 以当前柱子为主心骨，向左迭代寻找次级柱子
            temp = i - 1
            while temp >= 0 and heights[temp] >= heights[i]:
                # 当左侧的柱子持续较高时，尝试这个高柱子自己的次级柱子（DP
                temp = min_left_index[temp]
            # 当找到左侧矮一级的目标柱子时
            min_left_index[i] = temp
        
        # 记录每个柱子的右侧第一个矮一级的柱子的下标
        min_right_index[size-1] = size  # 初始化防止while死循环
        for i in range(size-2, -1, -1):
            # 以当前柱子为主心骨，向右迭代寻找次级柱子
            temp = i + 1
            while temp < size and heights[temp] >= heights[i]:
                # 当右侧的柱子持续较高时，尝试这个高柱子自己的次级柱子（DP
                temp = min_right_index[temp]
            # 当找到右侧矮一级的目标柱子时
            min_right_index[i] = temp
        
        for i in range(size):
            area = heights[i] * (min_right_index[i] - min_left_index[i] - 1)
            result = max(area, result)
        
        return result

# solution 2
# 单调栈
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        # Monotonic Stack
        '''
        找每个柱子左右侧的第一个高度值小于该柱子的柱子
        单调栈：栈顶到栈底：从大到小（每插入一个新的小数值时，都要弹出先前的大数值）
        栈顶，栈顶的下一个元素，即将入栈的元素：这三个元素组成了最大面积的高度和宽度
        情况一：当前遍历的元素heights[i]大于栈顶元素的情况
        情况二：当前遍历的元素heights[i]等于栈顶元素的情况
        情况三：当前遍历的元素heights[i]小于栈顶元素的情况
        '''

        # 输入数组首尾各补上一个0（与42.接雨水不同的是，本题原首尾的两个柱子可以作为核心柱进行最大面积尝试
        heights.insert(0, 0)
        heights.append(0)
        stack = [0]
        result = 0
        for i in range(1, len(heights)):
            # 情况一
            if heights[i] > heights[stack[-1]]:
                stack.append(i)
            # 情况二
            elif heights[i] == heights[stack[-1]]:
                stack.pop()
                stack.append(i)
            # 情况三
            else:
                # 抛出所有较高的柱子
                while stack and heights[i] < heights[stack[-1]]:
                    # 栈顶就是中间的柱子，主心骨
                    mid_index = stack[-1]
                    stack.pop()
                    if stack:
                        left_index = stack[-1]
                        right_index = i
                        width = right_index - left_index - 1
                        height = heights[mid_index]
                        result = max(result, width * height)
                stack.append(i)
        return result
```
Time: **O(n)**                   
Space: **O(n)** 

