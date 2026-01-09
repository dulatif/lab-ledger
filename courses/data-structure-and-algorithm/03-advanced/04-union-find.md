# Lesson 4: Disjoint Set (Union-Find)

## Learning Goals

- Implement `find` and `union`.
- Understand Path Compression and Rank optimization.
- Detect cycles in undirected graphs.

## 1. The Problem

You have elements `1, 2, 3, 4, 5`.

- Group `1` and `2`.
- Group `2` and `3`.
- **Question**: Are `1` and `3` connected? Yes (Transitive).

## 2. Implementation (The Library Code)

Memorize this class. It solves "Connected Components" instantly.

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, p):
        # Path Compression: Point directly to the ultimate root
        if self.parent[p] != p:
            self.parent[p] = self.find(self.parent[p])
        return self.parent[p]

    def union(self, p, q):
        rootP = self.find(p)
        rootQ = self.find(q)

        if rootP == rootQ: return False # Already connected

        # Union by Rank: Attach small tree to big tree
        if self.rank[rootP] > self.rank[rootQ]:
            self.parent[rootQ] = rootP
        elif self.rank[rootP] < self.rank[rootQ]:
            self.parent[rootP] = rootQ
        else:
            self.parent[rootQ] = rootP
            self.rank[rootP] += 1
        return True
```

## 3. Complexity

- **Time**: `O(alpha(n))`. Alpha is the Inverse Ackermann function.
- For all practical universe sizes, `alpha(n) < 5`.
- Essentially **Constant Time** `O(1)`.

## Key Takeaways

- Use Union-Find for **Network Connectivity** or **Kruskal's Algorithm**.
- **Path Compression** flattens the tree, making subsequent lookups instant.
