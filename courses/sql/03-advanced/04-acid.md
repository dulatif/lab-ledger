# 4. ACID Properties and Transactions

## 🎯 Learning Goal

Data safety.

## 🧠 Concept

**Transaction**: Single unit of work.

- **Atomicity**: All or Nothing.
- **Consistency**: DB stays valid (Constraints).
- **Isolation**: Transactions don't interfere.
- **Durability**: Saved forever once committed.

## 💻 Implementation

Banking: Transfer Money = Deduct A + Add B.
If Deduct happens but Add fails -> Data loss.
Transaction ensures both happen or neither does.

## 🧩 Activity / Challenge

1.  What implies Durability? (Write ahead logging to disk).

## 🔑 Key Takeaways

- The definition of "Reliable".
