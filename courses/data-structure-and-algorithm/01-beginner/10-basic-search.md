# Lesson 10: Basic Search

## Learning Goals

- Implement Linear Search.
- Implement Binary Search (Iterative).
- Understand the precondition for Binary Search.

## 1. Linear Search

Check every item until you find it.

- **Time**: `O(n)`
- **Data**: Unsorted.

```python
def linear_search(arr, target):
    for i, val in enumerate(arr):
        if val == target:
            return i
    return -1
```

## 2. Binary Search

Divide and Conquer.

- **Time**: `O(log n)`
- **Data**: **MUST BE SORTED**.

**Logic**:

1.  Check middle.
2.  If target < middle: Go Left.
3.  If target > middle: Go Right.

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = (left + right) // 2  # Integer division

        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1  # Discard left half
        else:
            right = mid - 1 # Discard right half

    return -1
```

## 3. The "Overflow" Bug

In languages like Java/C++, `(left + right)` can overflow integer limits.
The safe way is: `left + (right - left) // 2`.
Python handles large integers automatically, so `(left + right) // 2` is safe, but it's good to know the safe formula for interviews.

## 4. Bisect Module

Python has a built-in library for binary search.

```python
import bisect

arr = [1, 3, 5, 7]
# bisect_left returns the insertion point to maintain order
idx = bisect.bisect_left(arr, 5)
print(idx) # 2 (Found at index 2)
```

## Key Takeaways

- **Linear Search**: Simple, works on anything.
- **Binary Search**: Fast (`log n`), but data **must be sorted**.
- **Template**: Memorize the `while left <= right` template. Off-by-one errors are common here.
