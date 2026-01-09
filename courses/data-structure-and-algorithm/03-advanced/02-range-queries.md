# Lesson 2: Range Queries (Segment & Fenwick Trees)

## Learning Goals

- Solve "Sum of range [L, R]" efficiently.
- Update values efficiently.
- Segment Tree vs Prefix Sum.

## 1. The Problem

Given array `arr`, we want:

1.  `query(L, R)`: Sum elements from index L to R.
2.  `update(i, val)`: Change `arr[i]` to `val`.

| Method           | Query Time | Update Time            |
| :--------------- | :--------- | :--------------------- |
| Loop             | `O(n)`     | `O(1)`                 |
| Prefix Sum Array | `O(1)`     | `O(n)` (Rebuild array) |
| **Segment Tree** | `O(log n)` | `O(log n)`             |

## 2. Segment Tree

A binary tree where each node represents a **range**.

- Leaf: Single element `[i, i]`.
- Root: Entire array `[0, n-1]`.
- Parent `[L, R]` = Sum of Left Child `[L, Mid]` + Right Child `[Mid+1, R]`.

**Logic**: To find `Sum(2, 5)`, we combine the pre-calculated sums of the relevant nodes covering that range.

## 3. Fenwick Tree (Binary Indexed Tree)

A clever bit-manipulation data structure.

- Uses less space than Segment Tree (Same array size).
- Harder to understand, easier to code (10 lines).

```python
# Update Operation
def update(i, delta):
    while i < len(BIT):
        BIT[i] += delta
        i += i & (-i) # Add Least Significant Bit

# Query Prefix Sum
def query(i):
    s = 0
    while i > 0:
        s += BIT[i]
        i -= i & (-i) # Subtract LSB
    return s
```

## Key Takeaways

- **Static Array**: Use **Prefix Sums** (`acc = [1, 3, 6...]`).
- **Dynamic Array (Updates)**: Use **Segment Tree** or **Fenwick Tree**.
- Complexity: `O(log n)` for both query and update.
