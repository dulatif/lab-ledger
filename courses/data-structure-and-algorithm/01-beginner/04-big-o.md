# Lesson 4: Big-O Notation

## Learning Goals

- Analyze code complexity theoretically.
- Understand Worst-Case, Best-Case, and Average-Case.
- Ignore constants and lower-order terms.

## 1. Measuring Efficiency

We cannot measure speed in "seconds", because that depends on your computer (CPU speed, background tasks).
We measure speed in **Operations** relative to the **Input Size (n)**.

## 2. Big-O Rules

We care about how the runtime grows as `n` gets huge (1 Million+).

1.  **Drop Constants**: `O(2n)` becomes `O(n)`.
    - Looping 2 times or 1 time doesn't change the curve shape.
2.  **Drop Lower Order Terms**: `O(n^2 + n + 5)` becomes `O(n^2)`.
    - As `n` approaches infinity, `n^2` dominates everything else.

## 3. Examples

**O(1) - Constant Time**:

```python
def get_first(arr):
    return arr[0]
```

No matter if `arr` has 10 items or 1 billion, this takes 1 step.

**O(n) - Linear Time**:

```python
def find_sum(arr):
    total = 0
    for num in arr:  # Runs n times
        total += num
    return total
```

**O(n^2) - Quadratic Time**:

```python
def print_pairs(arr):
    for i in arr:          # n times
        for j in arr:      # n times
            print(i, j)
```

Nested loops are the hallmark of `O(n^2)`.

## 4. Space Complexity

This follows the same logic but for memory usage.

- Creating a loop variable `i`: `O(1)` space.
- Creating a new list `copy = arr[:]`: `O(n)` space.

## Key Takeaways

- **Input Size (n)**: The variable that determines how much work you do.
- **Worst Case**: Big-O usually describes the worst-case scenario (e.g., searching an array where the item is at the very end).
- **Loops**:
  - 1 Loop = `O(n)`
  - 2 Nested Loops = `O(n^2)`
  - 3 Nested Loops = `O(n^3)`
