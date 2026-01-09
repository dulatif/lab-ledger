# Lesson 8: Profiling & Debugging

## Learning Goals

- Use Flipper (or Expo Tools).
- Analyze FPS (Frames Per Second).

## 1. Flipper

Facebook's debugging tool.

- **Logs**: See native logs.
- **Network**: Inspect Native Network traffic (standard Chrome debugger usually misses Native Fetch calls).
- **React DevTools**: Inspect the component tree.

## 2. Performance Menu

Shake your device -> "Show Perf Monitor".

- **RAM**: Watch for leaks (continuously increasing).
- **UI JS**: You want solid **60 FPS**. If UI FPS drops, native thread is blocked. If JS FPS drops, your logic is slow.

## 3. React DevTools "Highlight Updates"

Enable this to see components flash when they re-render. If a component flashes but the data didn't change, you need `React.memo`!

## Key Takeaways

- **Console.log** is fine for logic.
- **Perf Monitor** is needed for "Jank" (stuttering).
- **Network Inspector** is crucial for debugging API calls.
