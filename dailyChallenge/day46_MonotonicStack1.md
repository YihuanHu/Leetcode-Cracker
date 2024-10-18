# Day46 Monotonic Stack #1
## Monotonic Stack
- when to use monotonic stack?
  - **when dealing with a one-dimensional array and we need to find the position of the first element to the right or left that is either larger or smaller than the current element**
- Time complexity is O(n).
  - Sacrifice space for quick time. We use a stack to keep track of the elements we’ve traversed. When we iterate through the array, we don’t know which elements we’ve previously encountered
- What elements should be stored in the monotonic stack?
  - The monotonic stack only needs to store the index i of the element and directly access it with T[i]
- Should the elements in the stack be in increasing or decreasing order?
  - **from the top of the stack to the bottom**, if we are looking for the next greater element to the right, the monotonic stack should be increasing
- Logic for creating the M stack: current element T[i] and the top element T[st.top()]
  -  If T[i] < T[st.top()]
  -  If T[i] = T[st.top()]
  -  If T[i] > T[st.top()]

##  LC 739 daily-temperatures
[Link](https://leetcode.com/problems/daily-temperatures/description/)   
[Cousrse Link](https://programmercarl.com/0739.%E6%AF%8F%E6%97%A5%E6%B8%A9%E5%BA%A6.html)
- initial as 0
- Use increaseing stack (from top to bottom)
  -  If T[i] < T[st.top()], add to stack
  -  If T[i] = T[st.top()], add to stack
  -  If T[i] > T[st.top()], pop and calculate res till elements inside is larger than top

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        answer = [0]*len(temperatures)
        stack = [0]
        for i in range(1,len(temperatures)):
            # 情况一和情况二
            if temperatures[i]<=temperatures[stack[-1]]:
                stack.append(i)
            # 情况三
            else:
                while len(stack) != 0 and temperatures[i]>temperatures[stack[-1]]:
                    answer[stack[-1]]=i-stack[-1]
                    stack.pop()
                stack.append(i)

        return answer
```
Time: **O(n)**                                  
Space: **O(n)** 

##  LC 496 next-greater-element-i
[Link](https://leetcode.com/problems/next-greater-element-i/description/)   
[Cousrse Link](https://programmercarl.com/0503.%E4%B8%8B%E4%B8%80%E4%B8%AA%E6%9B%B4%E5%A4%A7%E5%85%83%E7%B4%A0II.html)
- initial as -1
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
