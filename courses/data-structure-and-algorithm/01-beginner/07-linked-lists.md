# Lesson 7: Linked Lists

## Learning Goals

- Implement a `ListNode` class.
- Traverse a Linked List.
- Insert and Delete nodes.

## 1. The Node Class

A Linked List is a chain of nodes. Each node holds **data** and a **reference** (pointer) to the next node.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

    def __repr__(self):
        return f"{self.val} -> {self.next}"
```

## 2. Traversal

You need a "runner" pointer. Never lose the `head` reference!

```python
def print_list(head: ListNode):
    current = head
    while current:  # While current is not None
        print(current.val)
        current = current.next
```

## 3. Operations

### Insertion at Head (O(1))

```python
new_node = ListNode(10)
new_node.next = head
head = new_node
```

### Deletion

To delete a node, you route the _previous_ node's pointer around it.

```python
# Delete node with val 5
dummy = ListNode(0, head) # Dummy simplifies edge cases (deleting head)
prev = dummy
current = head

while current:
    if current.val == 5:
        prev.next = current.next # Skip current
        break
    prev = current
    current = current.next

return dummy.next
```

## 4. The "Dummy Node" Trick

Edge cases (like deleting the head or inserting into an empty list) are annoying.
A **Dummy Head** allows you to treat the head like any other node.

```python
dummy = ListNode(-1)
tail = dummy
# Add items to tail...
tail.next = ListNode(1)
tail = tail.next
return dummy.next  # The real head
```

## Key Takeaways

- **Memory**: Nodes are scattered in memory, unlike Arrays (contiguous).
- **Traversal**: Always `O(n)`. No random access like `arr[5]`.
- **Dummy Node**: Your best friend for avoiding `if head is None` checks.
