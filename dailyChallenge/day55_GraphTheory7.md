# Day55 GraphTheory7 Minimum Spanning Tree (MST)

## Minimum Spanning Tree (MST)
- A Minimum Spanning Tree is essentially the smallest subgraph that connects all nodes, using the minimum possible total edge weight
- In a graph with 𝑛 nodes, we need n−1 edges to connect all nodes without any cycles, ensuring the minimum possible total weight
- Two algo:
  - Prim
  - Kruskal's

##  Prim Algo
[Course Link](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-prim.html#%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AF)
- Steps:
  - Select the Node Closest to the Current Tree: Find the node with the smallest distance from any node already in the MST.
  - Add the Closest Node to the Tree: Add this selected node to the MST, effectively marking it as part of the solution.
  - Update the Distances of Non-Tree Nodes: For each node not yet in the MST, update its minimum distance to any node within the MST (this is done by updating the minDist array)
- minDist: array for storing the Mini distance to the current MST for all the non-MST nodes
```python
# 接收输入
v, e = list(map(int, input().strip().split()))
# 按照常规的邻接矩阵存储图信息，不可达的初始化为10001
graph = [[10001] * (v+1) for _ in range(v+1)]
for _ in range(e):
    x, y, w = list(map(int, input().strip().split()))
    graph[x][y] = w
    graph[y][x] = w

# 定义加入生成树的标记数组和未加入生成树的最近距离
visited = [False] * (v + 1)
minDist = [10001] * (v + 1)

# 循环 n - 1 次，建立 n - 1 条边
# 从节点视角来看：每次选中一个节点加入树，更新剩余的节点到树的最短距离，
# 这一步其实蕴含了确定下一条选取的边，计入总路程 ans 的计算
for _ in range(1, v + 1):
    min_val = 10002
    cur = -1
    for j in range(1, v + 1):
        if visited[j] == False and minDist[j] < min_val:
            cur = j
            min_val = minDist[j]
    visited[cur] = True
    for j in range(1, v + 1):
        if visited[j] == False and minDist[j] > graph[cur][j]:
            minDist[j] = graph[cur][j]

ans = 0
for i in range(2, v + 1):
    ans += minDist[i]
print(ans)
def prim(v, e, edges):
    import sys
    import heapq

    # 初始化邻接矩阵，所有值初始化为一个大值，表示无穷大
    grid = [[10001] * (v + 1) for _ in range(v + 1)]

    # 读取边的信息并填充邻接矩阵
    for edge in edges:
        x, y, k = edge
        grid[x][y] = k
        grid[y][x] = k

    # 所有节点到最小生成树的最小距离
    minDist = [10001] * (v + 1)

    # 记录节点是否在树里
    isInTree = [False] * (v + 1)

    # Prim算法主循环
    for i in range(1, v):
        cur = -1
        minVal = sys.maxsize

        # 选择距离生成树最近的节点
        for j in range(1, v + 1):
            if not isInTree[j] and minDist[j] < minVal:
                minVal = minDist[j]
                cur = j

        # 将最近的节点加入生成树
        isInTree[cur] = True

        # 更新非生成树节点到生成树的距离
        for j in range(1, v + 1):
            if not isInTree[j] and grid[cur][j] < minDist[j]:
                minDist[j] = grid[cur][j]

    # 统计结果
    result = sum(minDist[2:v+1])
    return result

if __name__ == "__main__":
    import sys
    input = sys.stdin.read
    data = input().split()
    
    v = int(data[0])
    e = int(data[1])
    
    edges = []
    index = 2
    for _ in range(e):
        x = int(data[index])
        y = int(data[index + 1])
        k = int(data[index + 2])
        edges.append((x, y, k))
        index += 3

    result = prim(v, e, edges)
    print(result)

```
Time Complexity: **O(n^2)**

##   Kruskal's algo
[Cousrse Link](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-Kruskal.html#%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AF)
- more stricky to handle
