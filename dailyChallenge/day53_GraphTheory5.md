# Day53 GraphTheory5

##   Union-Find  / Disjoint Set Union (DSU)
[Course Link](https://www.programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E5%B9%B6%E6%9F%A5%E9%9B%86%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E8%83%8C%E6%99%AF)
- when to use:
  - To merge two elements into one set, you use the union operation
  - To check whether two elements are in the same set, you use the find operation
- functions:
  - find: gonna return the root
  - isSame: return if two nodes have the same root
  - join: find the root of u & v and update link by change find(u)  -> v
- path compression: making each node on the path point directly to the root by updating father[u] = find(father[u])
  -  flattening of the tree structure ensures that subsequent find operations for any of these nodes will take constant time
- Complexity
  - Space: **O(n)**
  - Time:between **O(logn)** and **O(1)** more close to constant when more nodes being added
- optimization
  - path compression: Optimizes the find operation by making all nodes on the path point directly to the root, reducing the tree height over time.
  - union by rank: Optimizes the union operation by ensuring that the smaller tree (by rank) is attached to the larger tree
```python

######## optimization 1: path compression
# n is the number of nodes, slightly larger than the actual number of nodes in the problem.
n = 1005
father = list(range(n))  # Initialize the parent array with each element being its own parent.

# Union-Find (Disjoint Set) initialization
def init():
    for i in range(n):
        father[i] = i  # Each node is its own parent initially.

# Find the root of the node u with path compression
def find(u):
    if u == father[u]:
        return u
    else:
        father[u] = find(father[u])  # Path compression
        return father[u]

# Check if u and v have the same root
def isSame(u, v):
    return find(u) == find(v)

# Union operation: join the sets containing u and v
def join(u, v):
    root_u = find(u)  # Find the root of u
    root_v = find(v)  # Find the root of v
    if root_u != root_v:
        father[root_v] = root_u  # Merge the sets by linking the root of v to the root of u

# Example usage
init()  # Initialize the union-find structure
join(1, 2)  # Join node 1 and node 2
join(3, 4)  # Join node 3 and node 4
print(isSame(1, 2))  # Output: True (1 and 2 are now in the same set)
print(isSame(1, 3))  # Output: False (1 and 3 are in different sets)

######## optimization 2: 
# n is the number of nodes, typically slightly larger than the actual number of nodes.
n = 1005
father = list(range(n))  # Initialize the parent array where each element is its own parent.
rank = [1] * n  # Initialize the rank (height) of each tree as 1.

# Union-Find (Disjoint Set) initialization
def init():
    for i in range(n):
        father[i] = i  # Each node starts as its own parent.
        rank[i] = 1  # Each node starts with a rank of 1.

# Find operation: find the root of node u without path compression
def find(u):
    if u == father[u]:
        return u
    else:
        return find(father[u])

# Check if u and v are in the same set (i.e., share the same root)
def isSame(u, v):
    return find(u) == find(v)

# Union operation: join the sets containing u and v using union by rank
def join(u, v):
    root_u = find(u)  # Find the root of u
    root_v = find(v)  # Find the root of v

    if root_u != root_v:  # Only join if they are in different sets
        # Union by rank: attach the shorter tree under the taller tree
        if rank[root_u] <= rank[root_v]:
            father[root_u] = root_v  # root_u's tree joins root_v's tree
        else:
            father[root_v] = root_u  # root_v's tree joins root_u's tree

        # If both trees have the same rank, increase the rank of the new root
        if rank[root_u] == rank[root_v]:
            rank[root_v] += 1

# Example usage
init()  # Initialize the union-find structure
join(1, 2)  # Join node 1 and node 2
join(3, 4)  # Join node 3 and node 4
join(2, 3)  # Join node 2 and node 3
print(isSame(1, 4))  # Output: True (1, 2, 3, and 4 are now in the same set)
print(isSame(1, 5))  # Output: False (1 and 5 are in different sets)

```


##  107. 寻找存在的路径
 [Cousrse Link](https://www.programmercarl.com/kamacoder/0107.%E5%AF%BB%E6%89%BE%E5%AD%98%E5%9C%A8%E7%9A%84%E8%B7%AF%E5%BE%84.html)
- typical union find problem
```python

class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size + 1))  # 初始化并查集

    def find(self, u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])  # 路径压缩
        return self.parent[u]

    def union(self, u, v):
        root_u = self.find(u)
        root_v = self.find(v)
        if root_u != root_v:
            self.parent[root_v] = root_u

    def is_same(self, u, v):
        return self.find(u) == self.find(v)


def main():
    import sys
    input = sys.stdin.read
    data = input().split()
    
    index = 0
    n = int(data[index])
    index += 1
    m = int(data[index])
    index += 1
    
    uf = UnionFind(n)
    
    for _ in range(m):
        s = int(data[index])
        index += 1
        t = int(data[index])
        index += 1
        uf.union(s, t)
    
    source = int(data[index])
    index += 1
    destination = int(data[index])
    
    if uf.is_same(source, destination):
        print(1)
    else:
        print(0)

if __name__ == "__main__":
    main()


```
