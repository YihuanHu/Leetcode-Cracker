# Day58 GraphTheory9
##  ??? dijkstra in depth
[Course Link](https://www.programmercarl.com/kamacoder/0047.%E5%8F%82%E4%BC%9Adijkstra%E5%A0%86.html)      


## Bellman_ford
[Cousrse Link](https://www.programmercarl.com/kamacoder/0094.%E5%9F%8E%E5%B8%82%E9%97%B4%E8%B4%A7%E7%89%A9%E8%BF%90%E8%BE%93I.html#%E6%80%9D%E8%B7%AF)       
- The **Bellman-Ford algorithm** is used to find the shortest paths from a single source vertex to all other vertices in a weighted graph. Unlike Dijkstra's algorithm, which only works with graphs that have non-negative edge weights, Bellman-Ford can handle **negative edge weights**. This feature allows it to detect negative weight cycles, where the total weight is negative, potentially leading to infinitely decreasing path weights.
- Key Properties
  - **Single Source Shortest Path**: Finds the shortest path from a single source to all other vertices.
  - **Handles Negative Weights**: Can operate with negative weights, which many other shortest-path algorithms cannot.
  - **Cycle Detection**: Detects if a graph contains a negative weight cycle.


-  Algorithm Overview: works by **repeatedly relaxing all edges**.
  - For a graph with \( V \) vertices, it performs \( V - 1 \) passes over all edges, updating distances as it finds shorter paths:
  - **Initialization**: Set the distance to the source vertex to 0 and all other vertices to infinity.
  - **Relaxation**: For each vertex, go through all edges. If the distance to a vertex \( v \) through an edge \( u \rightarrow v \) is shorter than the current known distance, update the distance to \( v \).
  - **Negative Cycle Check**: After \( V - 1 \) passes, make an additional pass over all edges. If any edge can still be relaxed, it indicates a negative weight cycle in the graph.

- core idea:
   - 对所有边松弛一次，相当于计算 起点到达 与起点一条边相连的节点的最短距离
   - 那对所有边松弛两次 可以得到与起点 两条边相连的节点的最短距离
   - 那对所有边松弛三次 可以得到与起点 三条边相连的节点的最短距离
   - 需要对所有边松弛几次才能得到 起点 到终点的最短距离 => V-1

```python
def main():
    n, m = map(int, input().strip().split())
    edges = []
    for _ in range(m):
        src, dest, weight = map(int, input().strip().split())
        edges.append([src, dest, weight])
    
    minDist = [float("inf")] * (n + 1)
    minDist[1] = 0  # 起点处距离为0
    
    for i in range(1, n):
        updated = False
        for src, dest, weight in edges:
            if minDist[src] != float("inf") and minDist[src] + weight < minDist[dest]:
                minDist[dest] = minDist[src] + weight
                updated = True
        if not updated:  # 若边不再更新，即停止回圈
            break
    
    if minDist[-1] == float("inf"):  # 返还终点权重
        return "unconnected"
    return minDist[-1]
    
if __name__ == "__main__":
    print(main())

```
**Time Complexity**: \( O(V \cdot E) \), where \( V \) is the number of vertices and \( E \) is the number of edges.
**Space Complexity**: \( O(V) \) for storing distances.
