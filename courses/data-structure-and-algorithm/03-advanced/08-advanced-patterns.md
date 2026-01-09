# Lesson 8: Advanced Pointers & Patterns

## Learning Goals

- Master Sliding Window.
- Master Two Pointers (Target Sum).
- Master Fast & Slow Pointers (Cycle Detection).

## 1. Sliding Window

Used for subarray problems. "Find the longest substring with..."

**Pattern**: Expand `right` pointer. If invalid, shrink `left` pointer.

```python
def length_of_longest_substring_two_distinct(s):
    window = {}
    max_len = 0
    left = 0

    for right, char in enumerate(s):
        window[char] = window.get(char, 0) + 1

        while len(window) > 2: # Invalid State
            left_char = s[left]
            window[left_char] -= 1
            if window[left_char] == 0:
                del window[left_char]
            left += 1

        max_len = max(max_len, right - left + 1)

    return max_len
```

## 2. Two Pointers (Sorted Arrays)

Find two numbers that add up to Target.
Since array is sorted:

- Sum < Target? Increase Left (need bigger number).
- Sum > Target? Decrease Right (need smaller number).

```python
def two_sum_sorted(arr, target):
    l, r = 0, len(arr) - 1
    while l < r:
        s = arr[l] + arr[r]
        if s == target: return [l, r]
        elif s < target: l += 1
        else: r -= 1
    return []
```

## 3. Fast & Slow Pointers (Floyd's Cycle Finding)

Detect a cycle in a Linked List.
Move `slow` by 1 step, `fast` by 2 steps. If they meet, there is a cycle.

```python
def has_cycle(head):
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

## Key Takeaways

- **Subarrays** = Sliding Window.
- **Sorted Array Search** = Two Pointers or Binary Search.
- **Linked List Cycles** = Fast/Slow Pointers.
