# Lesson 6: Algorithmic Paradigms

## Learning Goals

- Identify when to use Brute Force, Greedy, or Backtracking.
- Solve N-Queens (Backtracking).

## 1. Brute Force

Try every possibility.

- **Examples**: Generating all permutations `O(n!)`, trying every password.
- **Status**: Usually the starting point in an interview ("Naive Solution").

## 2. Greedy Algorithms

Make the locally optimal choice at each step.

- **Example**: Change making (US Coins). To make 70 cents, take 25 (remain 45) -> 25 (remain 20) -> 10 (remain 10) -> 10.
- **Risk**: Greedy doesn't always work (e.g., Knapsack Problem). It only works if the problem has "Optimal Substructure".

## 3. Backtracking

Brute Force + "Giving Up Early" (Pruning).
**Example**: N-Queens. Place a Queen. If it attacks another, remove it (backtrack) and try next spot.

```python
def solve_n_queens(n):
    res = []
    board = [['.'] * n for _ in range(n)]

    def backtrack(row, cols, diags, anti_diags):
        if row == n:
            res.append(["".join(r) for r in board])
            return

        for col in range(n):
            if col in cols or (row-col) in diags or (row+col) in anti_diags:
                continue

            # Place
            board[row][col] = 'Q'
            cols.add(col)
            diags.add(row-col)
            anti_diags.add(row+col)

            # Recurse
            backtrack(row + 1, cols, diags, anti_diags)

            # Remove (Backtrack)
            board[row][col] = '.'
            cols.remove(col)
            diags.remove(row-col)
            anti_diags.remove(row+col)

    backtrack(0, set(), set(), set())
    return res
```

## Key Takeaways

- **Backtracking** is simply DFS on a "State Space Tree".
- Always define your **Base Case** (Success) and **Pruning Logic** (Failure).
