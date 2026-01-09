# Lesson 6: Heaps (Priority Queues)

## Learning Goals

- Use Python's `heapq` module.
- Implement Min-Heap and Max-Heap logic.
- Solve "K-th Largest Element" problems.

## 1. The `heapq` Module

Python has a built-in Min-Heap implementation. It operates on a standard list.

```python
import heapq

nums = [3, 1, 4, 1, 5]
heapq.heapify(nums)  # O(n)
print(nums[0])       # 1 (Always the Smallest)

heapq.heappush(nums, 0) # O(log n)
smallest = heapq.heappop(nums) # O(log n)
```

## 2. Max-Heap Hack

Python only has Min-Heap.
**Hack**: Multiply numbers by `-1`.

- To store `[10, 5, 20]`: Store `[-10, -5, -20]`.
- Smallest is `-20`.
- Flip sign back when popping: `-(-20) = 20` (Largest).

## 3. Common Pattern: Top K Elements

Find the Top 3 largest numbers in a stream.
**Solution**: Keep a Min-Heap of size 3.
If a new number is larger than the Heap's minimum (root), create space by popping root and pushing new number. Be sure to keep the largest elements in the heap.

```python
def top_k(nums, k):
    heap = [] # Min Heap
    for n in nums:
        heapq.heappush(heap, n)
        if len(heap) > k:
            heapq.heappop(heap) # Remove the smallest of the bunch
    return heap # Contains the K largest elements
```

## Key Takeaways

- `heapq` is your best friend in interviews.
- **Access Min**: `O(1)`.
- **Insert/Pop**: `O(log n)`.
- Remember the **Negation Trick** for Max-Heaps.
