# Lesson 4: Binary Trees & BST

## Learning Goals

- Implement a Binary Search Tree (BST).
- Structure a `TreeNode`.
- Search and Insert in `O(h)`.

## 1. The TreeNode

Similar to a Linked List, but with two pointers.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

## 2. Binary Search Tree (BST) Property

- **Left** subtree Nodes < Root.
- **Right** subtree Nodes > Root.
- No duplicates (usually).

This property allows binary search behavior.

## 3. Operations

### Search

```python
def search(root, target):
    if not root or root.val == target:
        return root

    if target < root.val:
        return search(root.left, target)
    else:
        return search(root.right, target)
```

### Insert

We must maintain the BST property.

```python
def insert(root, val):
    if not root:
        return TreeNode(val)

    if val < root.val:
        root.left = insert(root.left, val)
    else:
        root.right = insert(root.right, val)
    return root
```

## 4. Complexity

- **Best Case (Balanced)**: `O(log n)` height.
- **Worst Case (Skewed/Line)**: `O(n)` height (like a Linked List).
- **Solution**: Self-Balancing trees (AVL/Red-Black) - covered in Advanced.

## Key Takeaways

- BSTs are fast for lookup/insert (`O(log n)`).
- Recursion is the natural way to process trees.
- Keep the logic: Left is smaller, Right is bigger.
