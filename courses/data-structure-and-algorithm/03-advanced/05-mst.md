# Lesson 5: Minimum Spanning Trees (MST)

## Learning Goals

- Understand "Spanning Tree" concept.
- Implement Kruskal's Algorithm (Greedy + Union-Find).
- Implement Prim's Algorithm (Greedy + Priority Queue).

## 1. What is an MST?

Given a connected, weighted, undirected graph, we want to select edges such that:

1.  All vertices are connected.
2.  The total weight is minimized.
3.  No cycles are formed.

**Use Case**: Laying cables to connect cities with minimum cost.

## 2. Kruskal's Algorithm

**Logic**: Sort all edges by weight. Pick the smallest edge. If it doesn't form a cycle, add it.

```python
# Uses Union-Find from Lesson 4
def kruskal(n, edges):
    # edges = [[u, v, w], ...]
    uf = UnionFind(n)
    edges.sort(key=lambda x: x[2]) # Sort by weight

    mst_weight = 0
    edges_count = 0

    for u, v, w in edges:
        if uf.union(u, v):
            mst_weight += w
            edges_count += 1
            if edges_count == n - 1: # Optimization
                break

    return mst_weight
```

- **Time**: `O(E log E)` (Sorting).

## 3. Prim's Algorithm

**Logic**: Start from node 0. Grow the tree by always picking the cheapest edge connecting "The Tree" to "The Unknown".

```python
import heapq

def prim(n, graph):
    # graph = {u: [(v, w), ...]}
    visited = set()
    min_heap = [(0, 0)] # (weight, node)
    mst_weight = 0

    while len(visited) < n and min_heap:
        weight, u = heapq.heappop(min_heap)

        if u in visited:
            continue

        visited.add(u)
        mst_weight += weight

        for v, w in graph[u]:
            if v not in visited:
                heapq.heappush(min_heap, (w, v))

    return mst_weight
```

- **Time**: `O(E log V)` (Heap operations).

## Key Takeaways

- **Kruskal's**: Best for Sparse Graphs. Easy to code if you have Union-Find.
- **Prim's**: Best for Dense Graphs. Similar to Dijkstra.
