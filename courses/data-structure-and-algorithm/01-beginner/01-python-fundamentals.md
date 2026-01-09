# Lesson 1: Python Setup & Fundamentals

## Learning Goals

- Verify Python environment for DSA.
- Review essential Python syntax for competitive programming.
- Understand List Comprehensions and Slicing.

## 1. Why Python for DSA?

Python is the most popular language for coding interviews because:

- **Concise**: You write less code (no boilerplate like `public static void main`).
- **Readable**: Reads like pseudocode.
- **Rich Standard Library**: Built-in support for Heaps, Hash Maps, and Big Integers.

## 2. Essential Syntax Refresher

We assume you know basic variables and loops. Here are the tools specific to solving algorithmic problems efficiently.

### Type Hinting (Modern Python)

In interview code, type hints help clarity.

```python
def add(a: int, b: int) -> int:
    return a + b
```

### List Comprehensions

Create lists in one line.

```python
# Bad
squares = []
for i in range(10):
    squares.append(i * i)

# Good
squares = [i * i for i in range(10)]

# Filtered
evens = [x for x in range(10) if x % 2 == 0]
```

### Slicing

Python lists/strings can be sliced `[start:end:step]`.

```python
arr = [0, 1, 2, 3, 4, 5]
print(arr[1:4])  # [1, 2, 3] (End is exclusive)
print(arr[::-1]) # [5, 4, 3, 2, 1, 0] (Reverse array)
```

### Multiple Assignment

Swap variables in one line!

```python
a, b = 5, 10
a, b = b, a  # a is 10, b is 5
```

## 3. Range & Enumerate

Don't use `i` manually if you don't have to.

```python
names = ["Alice", "Bob"]

# Need index and value?
for i, name in enumerate(names):
    print(f"Index {i}: {name}")
```

## Key Takeaways

- Use **List Comprehensions** for initializing arrays.
- Use **Slicing** (`[::-1]`) for reversing.
- Use **Enumerate** when you need both the index and the value.
