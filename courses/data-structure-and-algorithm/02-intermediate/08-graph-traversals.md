# Lesson 8: Graph Traversals

## Learning Goals

- Adapt BFS/DFS for Graphs (Handling Cycles).
- Find Connected Components.
- Detect Cycles.

## 1. The "Visited" Set

Unlike Trees, Graphs implement cycles (A -> B -> A). You MUST track visited nodes or you will loop forever.

```python
visited = set()
```

## 2. Graph DFS (Recursion)

Perfect for "Can I reach X from Y?" or "Detect Cycle".

```python
def dfs(node, graph, visited):
    if node in visited:
        return

    print(node)
    visited.add(node)

    for neighbor in graph[node]:
        dfs(neighbor, graph, visited)
```

## 3. Graph BFS (Queue)

Perfect for "Shortest Path in Unweighted Graph" or "Level by Level".

```python
from collections import deque

def bfs(start_node, graph):
    visited = set([start_node])
    queue = deque([start_node])

    while queue:
        node = queue.popleft()
        print(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

## 4. Connected Components

How many islands are there?
Iterate through all nodes. If unseen, run BFS/DFS and increment count.

```python
count = 0
visited = set()
for node in all_nodes:
    if node not in visited:
        dfs(node, graph, visited)
        count += 1
```

## Key Takeaways

- **Visited Set**: Mandatory for Graphs.
- **BFS**: Use for Shortest Path.
- **DFS**: Use for Exhaustive Search / Path Existence.
