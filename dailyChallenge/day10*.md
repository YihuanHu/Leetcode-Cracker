# Day10 Stack and Queue

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
Time: **O(n)** for pop    **O(1)** for all other
Space: **O(n)** for push and **O(1)** for all other



## Adds on
- [ ] KMP practice: [Link](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

- String Summary :[Link](https://programmercarl.com/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93.html#%E4%BB%80%E4%B9%88%E6%98%AF%E5%AD%97%E7%AC%A6%E4%B8%B2)
- 2 pointers Summary: [Link](https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html#%E6%95%B0%E7%BB%84%E7%AF%87)
