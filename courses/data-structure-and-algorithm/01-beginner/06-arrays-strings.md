# Lesson 6: Arrays & Strings

## Learning Goals

- Understand Python Lists (Dynamic Arrays).
- Understand Immutable Strings.
- Master String building patterns.

## 1. Python Lists (Dynamic Arrays)

In Python, `list` is a dynamic array.

- **Access**: `arr[i]` is `O(1)`.
- **Append**: `arr.append(x)` is amortized `O(1)`.
- **Insert/Delete**: `arr.insert(0, x)` or `arr.pop(0)` is `O(n)` because elements must shift.

### Pre-allocating

If you know the size, pre-allocate to avoid resizing overhead.

```python
# Create an array of size 10 initialized to 0
arr = [0] * 10
```

## 2. Strings are Immutable

You cannot change a character in a string.

```python
s = "hello"
s[0] = "H"  # TypeError!
```

To modify a string, you must create a _new_ string.

```python
s = "H" + s[1:]  # Creates a copy -> O(n)
```

## 3. Efficient String Building

Repeatedly adding strings in a loop is `O(n^2)` because of immutability.

```python
# BAD: O(n^2)
s = ""
for char in large_list:
    s += char
```

**Solution**: Use a list and `.join()`.

```python
# GOOD: O(n)
parts = []
for char in large_list:
    parts.append(char)
final_string = "".join(parts)
```

## 4. Two Common Patterns

### Two Pointers (Reverse)

```python
def reverse_string(s: list) -> None:
    left, right = 0, len(s) - 1
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
```

### Sliding Window

Used for finding subarrays (e.g., "Longest substring without repeating characters").

## Key Takeaways

- **Insert at end (`append`)**: Fast `O(1)`.
- **Insert at start (`insert`)**: Slow `O(n)`.
- **Strings are Immutable**: Use list + `.join()` for heavily modified strings.
