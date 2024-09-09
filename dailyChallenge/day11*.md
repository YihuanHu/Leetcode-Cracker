# Day11 Stack and Queue Part2
- Priority Queue:
  - min heap: from smallest to max. pop the smallest
  - max heap: reverse
  - functions:
      - `import heapq`
      - pop: `heapq.heappush(pq, (2, 'task2'))` => log(n)
      - push: `heapq.heappop(pq)` => log(n)
      - convert to heap: `heapq.heapify(nums)`

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


## LC 347 top-k-frequent-elements
[LC Link](https://leetcode.com/problems/top-k-frequent-elements/description/)   
[Cousrse Link](https://programmercarl.com/0347.%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0.html)  

- Use min-heap to pop the smallest and keep the more frequent nums


```python
 import heapq

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Step 1: Count the frequency of each element in nums
        frequency_map = {}  # This will store the count of each element in nums
        for i in range(len(nums)):
            frequency_map[nums[i]] = frequency_map.get(nums[i], 0) + 1
        
        # Step 2: Use a min-heap to keep track of the top k most frequent elements
        min_heap = []  # Initialize a min-heap of size k

        # Step 3: Add elements to the heap based on their frequency
        for key, freq in frequency_map.items():
            heapq.heappush(min_heap, (freq, key))  # Push (frequency, element) into the heap
            if len(min_heap) > k:  # If the heap size exceeds k, remove the smallest element
                heapq.heappop(min_heap)
        
        # Step 4: Extract the top k most frequent elements from the heap
        result = [0] * k  # Initialize the result array
        for i in range(k - 1, -1, -1):
            result[i] = heapq.heappop(min_heap)[1]  # Pop the element with the highest frequency
        return result

```
Time: **O(nlogn)** 
Space: **O(n)** 


## Adds on
- [ ] Stack & Queue Review [Link](https://programmercarl.com/%E6%A0%88%E4%B8%8E%E9%98%9F%E5%88%97%E6%80%BB%E7%BB%93.html#%E6%A0%88%E7%BB%8F%E5%85%B8%E9%A2%98%E7%9B%AE)
