# Day48 GraphTheory1
## Graph Theory 
[Link](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E8%BF%9E%E9%80%9A%E6%80%A7)
- terminology:
  - directed graphs
  - undirected graphs
- Degrees:
  - in an undirected graph, the degree of a node is the number of edges connected to that node
  - directed graph:
    - Out-degree: The number of edges that originate from a given node.
    - In-degree: The number of edges that point to a given node
- Connectivity:
  - if any two nodes are reachable from each other, we call it a connected graph
  - In a directed graph, if any two nodes are mutually reachable from each other, we call it a strongly connected graph
- How to avoid:
  - Naive storage 
  - Adjacency list: good for sparse graph / easy to iterate for each edge
  - Adjacency matrix: easy understand / fast / suitable for dense graph
```python
# List of edges (undirected graph)
edges = [
    ('A', 'B'),
    ('A', 'C'),
    ('B', 'D')
]

# Example of an undirected graph using an adjacency list
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A', 'D'],
    'D': ['B', 'C']
}

# Example of an undirected graph using an adjacency matrix
# Graph: A - B, A - C, B - D, C - D
graph = [
    #  A  B  C  D
    [ 0, 1, 1, 0 ],  # A
    [ 1, 0, 0, 1 ],  # B
    [ 1, 0, 0, 1 ],  # C
    [ 0, 1, 1, 0 ]   # D
]
```
- iteration:
  - BFS: iterate eveything for a node and move to the next node
  - DFS: go until you hit the ground and then go back to change direction(recursion) to last possible selection. 
## DFS
- code 


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
