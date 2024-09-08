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
Space: **O(1)**


## LC 225 implement-stack-using-queues
[LC Link](https://leetcode.com/problems/implement-stack-using-queues/description/)   
[Cousrse Link with Iteration](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)  
[Cousrse Link with DP](https://mp.weixin.qq.com/s/r9pbkMyFyMAvmkf4QnL-1g)  
- Why use KMP algo?
- What is prefix/next table?
- How to find out next table?
- How to use this table ?

```python
    def getNext(self, next: List[int], s: str) -> None:
        j = 0
        next[0] = 0
        for i in range(1, len(s)):
            while j > 0 and s[i] != s[j]:
                j = next[j - 1]
            if s[i] == s[j]:
                j += 1
            next[i] = j
    
    def strStr(self, haystack: str, needle: str) -> int:
        if len(needle) == 0:
            return 0
        next = [0] * len(needle)
        self.getNext(next, needle)
        j = 0
        for i in range(len(haystack)):
            while j > 0 and haystack[i] != needle[j]:
                j = next[j - 1]
            if haystack[i] == needle[j]:
                j += 1
            if j == len(needle):
                return i - len(needle) + 1
        return -1
```
Time: **O(m+n)**   
Space: **O(n)**  - prefix/next table



## Adds on
- [ ] KMP practice: [Link](https://programmercarl.com/0459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2.html)

- String Summary :[Link](https://programmercarl.com/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93.html#%E4%BB%80%E4%B9%88%E6%98%AF%E5%AD%97%E7%AC%A6%E4%B8%B2)
- 2 pointers Summary: [Link](https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html#%E6%95%B0%E7%BB%84%E7%AF%87)
