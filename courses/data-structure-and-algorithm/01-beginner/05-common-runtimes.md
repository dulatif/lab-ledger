# Lesson 5: Common Runtimes

## Learning Goals

- Visualize the ranking of complexities.
- Understand Logarithmic `O(log n)` and Factorial `O(n!)` time.

## 1. Complexity Ranking (Fastest to Slowest)

1.  **O(1)** - Constant (Hash Map lookup)
2.  **O(log n)** - Logarithmic (Binary Search)
3.  **O(n)** - Linear (For Loop)
4.  **O(n log n)** - Linearithmic (Merge Sort, Python `.sort()`)
5.  **O(n^2)** - Quadratic (Bubble Sort)
6.  **O(2^n)** - Exponential (Recursion without memoization)
7.  **O(n!)** - Factorial (Permutations)

## 2. Deep Dive: O(log n)

"Logarithmic" means the problem size gets cut in half at every step.

- Does `n = 100` take 100 steps? No.
- Is it > 50? No.
- Is it > 75? No.
  It takes very few steps. `log_2(1,000,000)` is only ~20.
  Basically, `O(log n)` is instantaneous for all intensive purposes.

## 3. Deep Dive: O(2^n)

"Exponential" adds a branch at every step.

- Fibonacci recursive: `fib(n) = fib(n-1) + fib(n-2)`.
- Each call spawns 2 more calls.
- If `n=30`, this takes billions of operations. Avoid this!

## 4. Analogy

If you are looking for a name in a phone book (1000 pages):

- **O(n)**: Start at page 1, check every name until you find it.
- **O(log n)**: Open the middle. Values are lower? Throw away right half. Repeat.
- **O(1)**: You somehow magically know exactly which page number it is on.

## Key Takeaways

- **Aim for O(n) or O(n log n)** in interviews.
- **O(n^2)** is often acceptable for "Easy" problems or small inputs (`n < 1000`).
- **O(2^n)** usually signals you need Dynamic Programming.
