# Day11 Stack and Queue Part2

## LC 150 evaluate-reverse-polish-notation
[LC Link](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)   
[Cousrse Link](https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html#%E6%80%9D%E8%B7%AF)
-  when should we do one division? see operand
-  what should we calculate?  store in a stack and pop the last two
-  Be careful about - and / since it has order
  
```python
   def evalRPN(self, tokens: List[str]) -> int:
        res = []

        for i in tokens:
            if i == "+":
                res.append(res.pop() + res.pop())
            elif i == "-":
                a, b = res.pop(), res.pop()
                res.append(b - a)
            elif i == "*":
                res.append(res.pop() * res.pop())
            elif i == "/":
                a, b = res.pop(), res.pop()
                res.append(int(b / a))
            else:
                res.append(int(i))

        return res[0]

```
Time: **O(n)** 
Space: **O(n)** 


## LC 239 sliding-window-maximum
[LC Link](https://leetcode.com/problems/sliding-window-maximum/description/)   
[Cousrse Link](https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)  

- Only maintain the max values
- Monotonic stack: monotonic increase/decrease
  - push: delete all the prev elements smaller than the curr
  - pop: only pop if this is curr max since all the other smaller has already been deleted when push
  - top: the max element is at the front

```python
from collections import deque

class MyQueue:
    # Monotonic queue (decreasing order)
    def __init__(self):
        self.queue = deque()  # Use deque for efficient pop from both ends
    
    def pop(self, value):
        # Pop from the front if it matches the value being removed from the window
        if self.queue and value == self.queue[0]:
            self.queue.popleft()
    
    def push(self, value):
        # Maintain decreasing order in the queue by popping smaller elements from the back
        while self.queue and value > self.queue[-1]:
            self.queue.pop()
        self.queue.append(value)
    
    def front(self):
        # The front of the queue is always the maximum value
        return self.queue[0]

class Solution:
    def maxSlidingWindow(self, nums, k):
        que = MyQueue()  # Initialize the monotonic queue
        result = []
        
        # Populate the first k elements
        for i in range(k):
            que.push(nums[i])
        result.append(que.front())  # Add the maximum of the first window
        
        # Process the remaining elements
        for i in range(k, len(nums)):
            que.pop(nums[i - k])  # Remove the element that is sliding out of the window
            que.push(nums[i])      # Add the new element to the window
            result.append(que.front())  # Append the current max to the result
        
        return result

```
Time: **O(n)** 
Space: **O(k)** for maintianing the window


## LC 20 valid-parentheses
[LC Link](https://leetcode.com/problems/valid-parentheses/description/)   
[Cousrse Link](https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)  

-  Use stack(save more space) or Queue
-  Consider three cases:
    - unmatch parenthsis
    - extra parenthesis
    - miss parenthesis

```python
    def isValid(self, s: str) -> bool:

        mapping = {
            '(': ')',
            '[': ']',
            '{': '}'
        }
        stack = []

        for i in s:
            if i in mapping.keys():
                stack.append(mapping[i])
            elif not stack or i != stack[-1]:
                return False
            else:
                stack.pop()
        
        return not stack
```
Time: **O(n)** 
Space: **O(1)** 


## LC 1047 remove-all-adjacent-duplicates-in-string
[LC Link](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/description/)   
[Cousrse Link](https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)  

```python
    def removeDuplicates(self, s: str) -> str:
        stack = []
        for i in s:
            if stack and stack[-1] == i:
                stack.pop()
            else:
                stack.append(i)

        return "".join(stack) 
```
Time: **O(n)** 
Space: **O(n)** 

## Adds on
