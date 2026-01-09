# Lesson 1: Elementary Sorts

## Learning Goals

- Implement Bubble Sort, Insertion Sort, and Selection Sort.
- Understand why they are `O(n^2)`.
- Recognize when Insertion Sort is actually useful (Small/Nearly sorted data).

## 1. Bubble Sort

Repeatedly swap adjacent elements if they are in the wrong order.
"Bubbles" the largest element to the end in each pass.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped: # Optimization: Stop if already sorted
            break
```

- **Time**: `O(n^2)`

## 2. Selection Sort

Find the minimum element in the remaining list and swap it to the front.

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
```

- **Time**: `O(n^2)`

## 3. Insertion Sort

Build the sorted array one item at a time. Pick an element and "insert" it into the correct position in the sorted part.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j] # Shift right
            j -= 1
        arr[j + 1] = key
```

- **Time**: `O(n^2)`
- **Best Case**: `O(n)` if already sorted.
- **Usage**: Very fast for small arrays (`n < 50`) or nearly sorted data.

## Key Takeaways

- These algorithms are conceptually simple but inefficient for large datasets.
- **Insertion Sort** is the hero of the `O(n^2)` sorts and is often used as a sub-routine in advanced sorts (like Timsort) for small chunks of data.
