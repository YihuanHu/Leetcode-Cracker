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
- Pattern:
  - parameters: if we need 2d array for all the paths/1 d array for single path, we can create a global variable 
  - termination condition: sometimes we don't need it since we handle it inside the recursion condition
  - Handle the logic for the current node: for loop for all the nodes connected to the current node
- Pseudocode
```
void dfs(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本节点所连接的其他节点) {
        处理节点;
        dfs(图，选择的节点); // 递归
        回溯，撤销处理结果
    }
}
```


##  LC 797 all-paths-from-source-to-target
[Link](https://leetcode.com/problems/all-paths-from-source-to-target/description/)   
[Cousrse Link](https://www.programmercarl.com/kamacoder/0098.%E6%89%80%E6%9C%89%E5%8F%AF%E8%BE%BE%E8%B7%AF%E5%BE%84.html)
- use ACM
- DFS

```python
# solution 1: adjacency matrix
def dfs(graph, x, n, path, result):
    if x == n:
        result.append(path.copy())
        return
    for i in range(1, n + 1):
        if graph[x][i] == 1:
            path.append(i)
            dfs(graph, i, n, path, result)
            path.pop()

def main():
    n, m = map(int, input().split())
    graph = [[0] * (n + 1) for _ in range(n + 1)]

    for _ in range(m):
        s, t = map(int, input().split())
        graph[s][t] = 1

    result = []
    dfs(graph, 1, n, [1], result)

    if not result:
        print(-1)
    else:
        for path in result:
            print(' '.join(map(str, path)))

if __name__ == "__main__":
    main()

# solution2 : adjacency list
from collections import defaultdict

result = []  # 收集符合条件的路径
path = []  # 1节点到终点的路径

def dfs(graph, x, n):
    if x == n:  # 找到符合条件的一条路径
        result.append(path.copy())
        return
    for i in graph[x]:  # 找到 x指向的节点
        path.append(i)  # 遍历到的节点加入到路径中来
        dfs(graph, i, n)  # 进入下一层递归
        path.pop()  # 回溯，撤销本节点

def main():
    n, m = map(int, input().split())

    graph = defaultdict(list)  # 邻接表
    for _ in range(m):
        s, t = map(int, input().split())
        graph[s].append(t)

    path.append(1)  # 无论什么路径已经是从1节点出发
    dfs(graph, 1, n)  # 开始遍历

    # 输出结果
    if not result:
        print(-1)
    for pa in result:
        print(' '.join(map(str, pa)))

if __name__ == "__main__":
    main()

```

## BFS
- suitable for finding the shortest path between two middle nodes
- we can use array to store and keep the same iteration direction every time. we can also use stack, one for clockweise and another for anti clockwise
- Pseudocode
```
from collections import deque

# BFS function for graph traversal
def bfs(graph, start):
    visited = set()  # Set to keep track of visited nodes
    queue = deque([start])  # Initialize the queue with the starting node
    
    visited.add(start)  # Mark the start node as visited

    while queue:
        node = queue.popleft()  # Dequeue the next node
        print(node)  # Process the current node (e.g., print or perform an operation)

        # Traverse all the neighbors of the current node
        for neighbor in graph[node]:
            if neighbor not in visited:  # If neighbor hasn't been visited
                visited.add(neighbor)  # Mark neighbor as visited
                queue.append(neighbor)  # Enqueue the neighbor
```
