# Lesson 1: Self-Balancing Trees

## Learning Goals

- Understand why BSTs degrade to `O(n)`.
- Concept of AVL and Red-Black Trees.
- Concept of B-Trees (Databases).

## 1. The Balance Problem

In a normal BST, inserting `[1, 2, 3, 4, 5]` creates a linked list (Skewed Tree).
Search becomes `O(n)`.
We want `O(log n)`, which requires the height to be minimal (~`log n`).

## 2. Rotations (The Fix)

Self-balancing trees use **Rotations** (Left/Right) to restructure the tree while maintaining the BST property.

- **AVL Tree**: Strictly balanced. Checks height difference at every node. Fast lookups, slower inserts.
- **Red-Black Tree**: Loosely balanced using "colors". Fast inserts, acceptable lookups. Used in Java `TreeMap`, C++ `std::map`.

## 3. Python Implementation?

Implementing a full AVL/RB tree in an interview is usually a **bad idea** (too much code).

- **Interview Strategy**: Use a standard BST or Hash Map. If you strictly need ordered keys + fast updates, mention you would use a "Balanced BST".
- There is no built-in Balanced BST in Python's standard library. (Java has `TreeMap`).

## 4. B-Trees (Database Indexes)

Binary Trees are bad for Disk Storage (too deep).
**B-Tree**: A "Fat" tree. Each node has thousands of children.

- Height is tiny (3-4 levels).
- Optimized for reading data blocks from a hard drive.
- Used by **PostgreSQL** and **MySQL**.

## Key Takeaways

- **AVL/Red-Black**: In-memory balanced trees. Guaranteed `O(log n)`.
- **B-Trees**: Disk-based balanced trees.
- **Interview Tip**: Know _why_ they exist, but don't memorize the rotation code unless specifically asked.
