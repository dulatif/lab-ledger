# Lesson 4: NumPy & Vectorization

## Learning Goals

- `ndarray`.
- Broadcasting.

## 1. Why NumPy?

Python lists are slow. NumPy arrays (`ndarray`) are fixed-type, contiguous memory blocks (C-like speed).

```python
import numpy as np
arr = np.array([1, 2, 3])
```

## 2. Vectorization

Applying an operation to the whole array at once (no `for` loops!).

```python
# Slow
for i in range(len(arr)):
    arr[i] *= 2

# Fast
arr = arr * 2
```

## 3. Broadcasting

Math between arrays of different shapes.
`[1, 2, 3] + 5` -> `[6, 7, 8]` (5 is broadcasted to match the shape).

## Key Takeaways

- Avoid `for` loops in Python whenever possible.
- Use Vectorization for 100x speedups.
