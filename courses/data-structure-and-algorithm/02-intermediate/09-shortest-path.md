# Lesson 9: Shortest Path (Dijkstra)

## Learning Goals

- Understand Weighted Graphs.
- Implement Dijkstra's Algorithm using `heapq`.

## 1. The Problem

In an unweighted graph, BFS gives the shortest path (fewest edges).
In a **weighted** graph (e.g., Road Map with distances), BFS fails. 3 short roads might be faster than 1 long highway.

## 2. Dijkstra's Algorithm

It's just BFS with a **Priority Queue** instead of a regular Queue.
Always explore the "cheapest" known node next.

**Data Structures**:

- `distances = {node: infinity}`
- `min_heap = [(0, start_node)]` (Tuple: `cost`, `node`)

## 3. Implementation

```python
import heapq

def dijkstra(start_node, graph):
    # graph = {u: [(v, weight), ...]}

    distances = {node: float('inf') for node in graph}
    distances[start_node] = 0

    pq = [(0, start_node)] # Min-Heap based on Cost

    while pq:
        current_dist, u = heapq.heappop(pq)

        # Optimization: If we found a shorter way to u already, skip
        if current_dist > distances[u]:
            continue

        for v, weight in graph[u]:
            distance = current_dist + weight

            # Key Logic: Relaxation
            if distance < distances[v]:
                distances[v] = distance
                heapq.heappush(pq, (distance, v))

    return distances
```

## 4. Complexity

- **Time**: `O(E * log V)`.
- **Use Case**: Google Maps, Network Routing.
- **Limitation**: Cannot handle Negative Weights (Use Bellman-Ford).

## Key Takeaways

- **Weighted Graph** = Dijkstra.
- **Priority Queue** ensures we always process the closest node.
- **Relaxation**: Updating a neighbor's distance if we found a shorter route.
