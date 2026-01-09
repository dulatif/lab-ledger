# Lesson 7: Graph Representations

## Learning Goals

- Understand Vertices (Nodes) and Edges.
- Implement Adjacency Matrix vs Adjacency List.
- Model Directed vs Undirected graphs.

## 1. Terminology

- **Vertex (V)**: A node.
- **Edge (E)**: A connection between nodes.
- **Weight**: Cost of the edge (optional).
- **Directed**: One-way street (A -> B).
- **Undirected**: Two-way street (A <-> B).

## 2. Adjacency Matrix

A 2D array where `matrix[i][j] = 1` if connected.

- **Space**: `O(V^2)` (Huge if sparse!)
- **Lookup**: `O(1)` (Is A connected to B?)

```python
# 3 Nodes (0, 1, 2)
# 0 -> 1, 1 -> 2
matrix = [
    [0, 1, 0], # Node 0
    [0, 0, 1], # Node 1
    [0, 0, 0]  # Node 2
]
```

## 3. Adjacency List (Standard)

A Dictionary used to store neighbors.

- **Space**: `O(V + E)` (Efficient).
- **Lookup**: `O(Degree)` (Slow to check strictly "Is A connected to B?").

```python
from collections import defaultdict

graph = defaultdict(list)

# Function to add edge
def add_edge(u, v, directed=False):
    graph[u].append(v)
    if not directed:
        graph[v].append(u)

add_edge(0, 1)
add_edge(1, 2)
# graph = {0: [1], 1: [0, 2], 2: [1]}
```

## Key Takeaways

- Always use **Adjacency Lists** for coding interviews (unless V is tiny and dense).
- Use a `defaultdict(list)` for easy implementation.
