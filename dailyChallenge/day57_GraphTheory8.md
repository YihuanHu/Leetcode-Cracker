# Day57 GraphTheory8 
##  Topological Sorting
[Course Link](https://www.programmercarl.com/kamacoder/0117.%E8%BD%AF%E4%BB%B6%E6%9E%84%E5%BB%BA.html#%E6%8B%93%E6%89%91%E6%8E%92%E5%BA%8F%E7%9A%84%E8%83%8C%E6%99%AF)
- Convert DAG to linear ordering
  - order vertices in a **Directed Acyclic Graph (DAG)** such that, for every directed edge \( u \rightarrow v \), vertex \( u \) comes before vertex \( v \) in the ordering. This method is highly applicable for scenarios with sequential dependencies, like project scheduling, course prerequisites, and build systems
- features:
  - **Applicable Only to DAGs**: Topological sorting is only possible on acyclic directed graphs. Any cycle would obstruct a linear ordering of dependencies.
  - **Non-unique Orderings**: Multiple valid topological orders can exist for the same DAG.
- steps for **Kahn’s Algorithm (BFS-Based)**
   - **Initialize** an in-degree array, with each node counting its incoming edges.
   - **Identify** nodes with an **in-degree of 0**, as these can be processed first.
   - **While** there are nodes with in-degree 0:
     - **Remove** the node by adding it to the topological order.
     - **Reduce** the in-degrees of its neighboring nodes.
     - **Add** any neighbor with an in-degree of 0 to the processing queue.
```python
from collections import deque, defaultdict

def topological_sort(n, edges):
    inDegree = [0] * n # inDegree 记录每个文件的入度
    umap = defaultdict(list) # 记录文件依赖关系

    # 构建图和入度表
    for s, t in edges:
        inDegree[t] += 1
        umap[s].append(t)

    # 初始化队列，加入所有入度为0的节点
    queue = deque([i for i in range(n) if inDegree[i] == 0])
    result = []

    while queue:
        cur = queue.popleft()  # 当前选中的文件
        result.append(cur)
        for file in umap[cur]:  # 获取该文件指向的文件
            inDegree[file] -= 1  # cur的指向的文件入度-1
            if inDegree[file] == 0:
                queue.append(file)

    if len(result) == n:
        print(" ".join(map(str, result)))
    else:
        print(-1)


if __name__ == "__main__":
    n, m = map(int, input().split())
    edges = [tuple(map(int, input().split())) for _ in range(m)]
    topological_sort(n, edges)

```
time complexity of \( O(V + E) \), where:
  - \( V \): Number of vertices
  - \( E \): Number of edges

##   Kruskal's algo
[Cousrse Link](https://www.programmercarl.com/kamacoder/0053.%E5%AF%BB%E5%AE%9D-Kruskal.html#%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AF)
- Steps:
  - Sort the edges by their weights, as we prioritize adding the smallest edges
  - iterate over the sorted edges:
    - If the two nodes at the ends of an edge belong to the same set, adding this edge would create a cycle in the graph, so we skip it
    - If the two nodes are in different sets, add the edge to the MST, and then unite the two nodes into the same set
```python
class Edge:
    def __init__(self, l, r, val):
        self.l = l
        self.r = r
        self.val = val

n = 10001
father = list(range(n))

def init():
    global father
    father = list(range(n))

def find(u):
    if u != father[u]:
        father[u] = find(father[u])
    return father[u]

def join(u, v):
    u = find(u)
    v = find(v)
    if u != v:
        father[v] = u

def kruskal(v, edges):
    edges.sort(key=lambda edge: edge.val)
    init()
    result_val = 0

    for edge in edges:
        x = find(edge.l)
        y = find(edge.r)
        if x != y:
            result_val += edge.val
            join(x, y)

    return result_val

if __name__ == "__main__":
    import sys
    input = sys.stdin.read
    data = input().split()

    v = int(data[0])
    e = int(data[1])

    edges = []
    index = 2
    for _ in range(e):
        v1 = int(data[index])
        v2 = int(data[index + 1])
        val = int(data[index + 2])
        edges.append(Edge(v1, v2, val))
        index += 3

    result_val = kruskal(v, edges)
    print(result_val)
```
Time Complexity **(logn*n)**  

## Adds On
- Kruskal is handling the edge while Prim is handling node
  - With a fixed number of nodes, the fewer the edges in the graph, the fewer edges Kruskal's algorithm needs to check. Conversely, Prim’s algorithm operates on nodes directly, so its efficiency increases with fewer nodes
  - Time Complexity:
    - Kruskal’s algorithm is preferable for sparse graphs, where the number of edges is low **O(logn*n)** 
    - Prim’s algorithm performs better on dense graphs, close to or equivalent to a complete graph (where all nodes are interconnected) **O(n^2)**
