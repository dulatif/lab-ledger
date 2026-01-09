# Lesson 9: Hash Tables

## Learning Goals

- Understand Hash Map properties (`dict`).
- Understand Hash Sent properties (`set`).
- Handle collisions conceptually.

## 1. The Magic of O(1)

Arrays map `Index (int) -> Value`.
Hash Maps map `Key (any) -> Value`.

It uses a **Hash Function** to convert the Key into an integer index.
`hash("apple") -> 1234 -> Index 4`.

**Operations**:

- Insert: `O(1)`
- Lookup: `O(1)`
- Delete: `O(1)`

## 2. Python Dictionary (`dict`)

```python
counts = {}

# Insert/Update
counts["apple"] = 1

# Check existence (O(1))
if "apple" in counts:
    print(counts["apple"])

# Iterate
for key, val in counts.items():
    print(f"{key}: {val}")
```

### DefaultDict

Avoids "KeyError" by providing a default value.

```python
from collections import defaultdict

# Default value is 0 (int)
counts = defaultdict(int)

words = ["apple", "banana", "apple"]
for w in words:
    counts[w] += 1  # No need to check if w exists first!
```

## 3. Python Set (`set`)

Like a dictionary, but only stores Keys (no values). Useful for checking "Have I seen this before?".

```python
seen = set()
arr = [1, 2, 3, 1]

for x in arr:
    if x in seen:
        print(f"Duplicate found: {x}")
    seen.add(x)
```

## 4. Hashable Keys

Keys must be **Immutable**.

- Valid: Strings, Numbers, Tuples `(1, 2)`.
- Invalid: Lists `[1, 2]`. You cannot use a mutable list as a dictionary key.

## Key Takeaways

- **Dicts** and **Sets** are the most powerful tools in DSA.
- Always consider a Hash Map if you need to optimize from `O(n)` scan to `O(1)` lookup.
- Use `defaultdict` to write cleaner counting logic.
