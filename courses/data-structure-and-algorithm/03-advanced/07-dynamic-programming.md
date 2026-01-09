# Lesson 7: Dynamic Programming (DP)

## Learning Goals

- Understand "Overlapping Subproblems".
- Top-Down (Memoization) vs Bottom-Up (Tabulation).
- Solve Fibonacci and Climbing Stairs.

## 1. The Core Concept

DP is just **Recursion + Caching**.
If a recursive tree calculates `fib(5)` twice, we are wasting time. Store the result!

## 2. Top-Down (Memoization)

Write the recursive solution, then add a dictionary `memo`.

```python
memo = {}

def fib(n):
    if n <= 1: return n
    if n in memo: return memo[n]

    memo[n] = fib(n-1) + fib(n-2)
    return memo[n]
```

- **Time**: `O(n)`. Without memo, it was `O(2^n)`.

## 3. Bottom-Up (Tabulation)

Iterative. Fill a table from 0 up to n.

```python
def fib_iterative(n):
    if n <= 1: return n
    dp = [0] * (n + 1)
    dp[1] = 1

    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]

    return dp[n]
```

- **Space Optimization**: You only need the last 2 numbers, not the whole array.

## 4. Key Problem: Climbing Stairs

You can climb 1 or 2 steps. How many ways to reach top `n`?

- To reach step `n`, you came from `n-1` OR `n-2`.
- `ways(n) = ways(n-1) + ways(n-2)`.
- It's literally Fibonacci!

## Key Takeaways

- **Identify DP**: If the problem asks for "Minimum", "Maximum", or "Number of ways", it's likely DP.
- **Memoization** is easier to write in interviews.
- **Tabulation** is safer (no Stack Overflow).
