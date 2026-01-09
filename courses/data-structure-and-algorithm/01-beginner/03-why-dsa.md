# Lesson 3: Why Data Structures?

## Learning Goals

- Differentiate between Data Structures and Abstract Data Types (ADT).
- Understand the "Trade-off" principle.

## 1. What is a Data Structure?

A way of organizing and storing data so it can be accessed and modified efficiently.

- **Example**: An array stores data contiguously. A linked list stores data via pointers.

## 2. ADT vs Implementation

- **Abstract Data Type (ADT)**: The _conceptual_ model. It defines **what** operations exist (e.g., "A Stack allows Push and Pop"). It does not define how they work.
- **Data Structure**: The _concrete_ implementation. (e.g., "This Stack is built using a Linked List").

| ADT   | Possible Implementations                   |
| :---- | :----------------------------------------- |
| List  | Dynamic Array, Linked List                 |
| Stack | Array, Linked List                         |
| Map   | Hash Table, Balanced Search Tree (TreeMap) |

## 3. The Trade-Off Principle

There is no "Perfect Data Structure". Every structure optimizes one thing at the expense of another.

- **Array**:
  - _Pro_: Instant access to index 500 (`O(1)`).
  - _Con_: Slow to insert at index 0 (`O(n)` - must shift everyone else).
- **Linked List**:
  - _Pro_: Instant insertion at index 0 (`O(1)`).
  - _Con_: Slow to access index 500 (`O(n)` - must walk the chain).

**Your Job as an Engineer**: Choose the right structure for the problem based on these trade-offs.

## Key Takeaways

- **ADT** = Interface (What).
- **Implementation** = Code (How).
- **No Free Lunch**: You usually trade **Space** (memory) for **Time** (speed), or Read speed for Write speed.
