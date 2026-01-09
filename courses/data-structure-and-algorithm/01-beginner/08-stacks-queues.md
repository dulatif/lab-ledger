# Lesson 8: Stacks & Queues

## Learning Goals

- Implement Stack (LIFO) using `list`.
- Implement Queue (FIFO) using `collections.deque`.

## 1. Stack (LIFO - Last In, First Out)

Think of a stack of plates. You add to the top and take from the top.

**Use Case**: Undo button, Browser History, DFS.

**In Python**: Just use a `list`.

```python
stack = []

# Push (O(1))
stack.append(1)
stack.append(2)

# Pop (O(1))
val = stack.pop() # Returns 2

# Peek (O(1))
top = stack[-1]
```

## 2. Queue (FIFO - First In, First Out)

Think of a line at a store. People join the back, leave the front.

**Use Case**: Printer jobs, BFS.

**In Python**: Do NOT use `list`. Popping from the front (`list.pop(0)`) is `O(n)`.
Use `collections.deque` (Double-Ended Queue).

```python
from collections import deque

queue = deque()

# Enqueue (O(1))
queue.append(1)
queue.append(2)

# Dequeue (O(1))
val = queue.popleft() # Returns 1
```

## 3. Monotonic Stack (Pattern)

A common advanced pattern: Keep elements in the stack sorted.

- **Problem**: Find the next greater element for each number.
- **Logic**: If current number > stack top, pop stack (we found the next greater for that popped item).

## Key Takeaways

- **Stack**: Use `list`. Key methods: `append()`, `pop()`.
- **Queue**: Use `deque`. Key methods: `append()`, `popleft()`.
- **Efficiency**: Both structures provide `O(1)` add/remove operations.
