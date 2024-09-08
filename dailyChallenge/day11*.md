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

- Priority Queue

```python
from collections import deque

class MyStack:
    def __init__(self):
        self.q = deque()
        self.top_elem = 0

    """
    Add an element to the top of the stack.
    """
    def push(self, x: int) -> None:
        # x is added to the back of the queue but is the top of the stack.
        self.q.append(x)
        self.top_elem = x

    """
    Return the top element of the stack.
    """
    def top(self) -> int:
        return self.top_elem

    """
    Remove and return the top element of the stack.
    """
    def pop(self) -> int:
        size = len(self.q)
        # Leave the last 2 elements in the queue
        while size > 2:
            self.q.append(self.q.popleft())
            size -= 1
        # Record the new top element
        self.top_elem = self.q[0]
        self.q.append(self.q.popleft())
        # Remove the previous top element
        return self.q.popleft()

    """
    Check if the stack is empty.
    """
    def empty(self) -> bool:
        return len(self.q) == 0

```
Time: **O(n)** for pop    **O(1)** for all other
Space: **O(n)** for push and **O(1)** for all other



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
