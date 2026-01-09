# Lesson 3: Heap Sort

## Learning Goals

- Understand the Binary Heap structure using an Array.
- Implement `heapify`.
- Perform Heapsort.

## 1. What is a Heap?

A binary tree where every parent is greater than its children (Max-Heap).
It is stored in an array:

- Parent `i` -> Children `2*i + 1` and `2*i + 2`.

## 2. Heapify

The process of creating a heap from an unsorted array.
We "sift down" elements starting from the bottom-most parents.

## 3. The Algorithm

1.  Build Max-Heap from the array.
2.  Swap Root (Largest) with Last Element.
3.  Reduce heap size by 1.
4.  Heapify Root.
5.  Repeat until size is 1.

```python
def heapify(arr, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2

    if left < n and arr[left] > arr[largest]:
        largest = left

    if right < n and arr[right] > arr[largest]:
        largest = right

    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

def heap_sort(arr):
    n = len(arr)
    # Build Max Heap (Starting from last parent)
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # Extract elements
    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i] # Swap root to end
        heapify(arr, i, 0) # Fix root
```

## Key Takeaways

- **Space**: `O(1)` (In-place).
- **Time**: `O(n log n)` worst case.
- **Comparison**: Slower than Quick Sort in practice, but guarantees `O(n log n)` unlike Quick Sort's worst case.
- **Priority Queues**: Heaps are mostly used for Priority Queues, not just sorting.
