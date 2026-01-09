# Lesson 2: Efficient Sorts (Merge & Quick)

## Learning Goals

- Understand Divide & Conquer strategy.
- Implement Merge Sort (`O(n log n)` stable).
- Implement Quick Sort (`O(n log n)` average, in-place).

## 1. Merge Sort

Divide the list in half recursively until single elements remain. Then merge the sorted halves back together.

**Concept**:
`[3, 1, 4, 1]` -> `[3, 1]` `[4, 1]` -> `[1, 3]` `[1, 4]` -> `[1, 1, 3, 4]`

```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        L = arr[:mid]
        R = arr[mid:]

        merge_sort(L)
        merge_sort(R)

        i = j = k = 0
        # Check if L[i] or R[j] is smaller
        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                arr[k] = L[i]
                i += 1
            else:
                arr[k] = R[j]
                j += 1
            k += 1

        # Check for remaining elements
        while i < len(L):
            arr[k] = L[i]; i += 1; k += 1
        while j < len(R):
            arr[k] = R[j]; j += 1; k += 1
```

- **Time**: `O(n log n)` always.
- **Space**: `O(n)` (Needs extra arrays).

## 2. Quick Sort

Pick a **Pivot**. Partition the array so small elements are on Left, large on Right. Recurse.

```python
def quick_sort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1
```

- **Time**: `O(n log n)` average. `O(n^2)` worst case (if sorted and pivot is bad).
- **Space**: `O(log n)` stack space.

## Key Takeaways

- **Merge Sort**: Guarantees `O(n log n)` but needs memory. Use for Linked Lists.
- **Quick Sort**: Faster in practice (cache locality) and in-place. Standard for Arrays.
- **Timsort**: Python's `.sort()` uses Timsort (Merge Sort + Insertion Sort).
