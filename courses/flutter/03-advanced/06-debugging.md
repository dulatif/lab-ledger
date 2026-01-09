# Lesson 6: Debugging & Profiling

## Learning Goals

- Flutter Inspector.
- Performance Overlay.
- DevTools.

## 1. Flutter Inspector

Visualizes the widget tree.
"Select Widget Mode": Click on the simulator to find the code.
"Debug Paint": Draws boxes around everything.

## 2. Performance Overlay

Shows GPU and UI Thread time (ms).
Rule: Keep both < 16ms (60fps).

## 3. Memory Leaks

Open DevTools -> Memory.
Take a snapshot. Navigate. GC. Take another snapshot.
If object count grows indefinitely, you have a leak (usually an unclosed Stream or Controller).

## Key Takeaways

- Don't guess why it's slow. Measure it.
- Fix `Overflow` errors immediately.
