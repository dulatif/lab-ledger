# Lesson 5: Tree Traversals

## Learning Goals

- Master DFS (In-order, Pre-order, Post-order).
- Master BFS (Level-order).
- Know when to use which.

## 1. Depth First Search (DFS)

Go deep effectively using recursion (Stack).

### In-Order

`Left -> Root -> Right`

- **Use Case**: Returns BST values in sorted order.

```python
def inorder(root):
    if not root: return
    inorder(root.left)
    print(root.val)
    inorder(root.right)
```

### Pre-Order

`Root -> Left -> Right`

- **Use Case**: Copying a tree (Since you need root first).

### Post-Order

`Left -> Right -> Root`

- **Use Case**: Deleting a tree (Delete children before parent).

## 2. Breadth First Search (BFS) / Level Order

Visit row by row. Uses a **Queue**.

```python
from collections import deque

def bfs(root):
    if not root: return
    queue = deque([root])

    while queue:
        node = queue.popleft()
        print(node.val)

        if node.left: queue.append(node.left)
        if node.right: queue.append(node.right)
```

## 3. Iterative DFS

Sometimes you can't use recursion (Stack Overflow). Use an explicit stack.

```python
def iterative_preorder(root):
    stack = [root]
    while stack:
        node = stack.pop()
        print(node.val)
        # Push Right first so Left is processed first (LIFO)
        if node.right: stack.append(node.right)
        if node.left: stack.append(node.left)
```

## Key Takeaways

- **In-Order** on BST = Sorted List.
- **BFS** finds the shortest path in an unweighted tree/graph (closest node to root).
- Iterative DFS mimics the call stack.
