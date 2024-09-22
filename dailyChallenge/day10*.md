# Day10 Stack and Queue
# Stack and Queue
- Both are container adapter: can use diff containers(vector, dequeue, list)
- Both are push/pop/top for O(1)

## LC 232 implement-queue-using-stacks
[LC Link](https://leetcode.com/problems/implement-queue-using-stacks/description/)   
[Cousrse Link](https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE)
- Use two stacks:
    - one for input: append when push
    - one for output: reverse the order when pop and only add new element if output is empty to keep the order
- **Copy one stack into another reverse the order while queue doesn't**
  
```python
class MyQueue:

    def __init__(self):
        self.s1 = []
        self.s2 = []

    """
    Add an element to the end of the queue.
    """
    def push(self, x: int) -> None:
        self.s1.append(x)

    """
    Remove and return the element at the front of the queue.
    """
    def pop(self) -> int:
        # Call peek first to ensure s2 is not empty.
        self.peek()
        return self.s2.pop()

    """
    Return the element at the front of the queue.
    """
    def peek(self) -> int:
        if not self.s2:
            # Move elements from s1 to s2.
            while self.s1:
                self.s2.append(self.s1.pop())
        return self.s2[-1]

    """
    Check if the queue is empty.
    """
    def empty(self) -> bool:
        return not self.s1 and not self.s2


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()

```
Time: **O(n)** for peek and pop     **O(1)** for push and empty 
Space: **O(n)** for push and **O(1)** for all other


## LC 225 implement-stack-using-queues
[LC Link](https://leetcode.com/problems/implement-stack-using-queues/description/)   
[Cousrse Link](https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)  

- Pop only needs to find the topelement and remain all the elements in the same order by re-join the whole queue except the last two one
- Not use Python list since it takes O(n) to pop(0)

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
Time: **O(n * 2^n)** more like creating subset from n elemets
Space: **O(n)** 



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
