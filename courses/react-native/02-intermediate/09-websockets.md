# Lesson 9: Real-time Updates (WebSockets)

## Learning Goals

- Implement WebSockets in React Native.
- Handle connection lifecycle.

## 1. WebSocket API

React Native supports the standard WebSocket API.

```jsx
useEffect(() => {
  const ws = new WebSocket("wss://echo.websocket.org");

  ws.onopen = () => {
    // Connection opened
    ws.send("Hello Server!");
  };

  ws.onmessage = (e) => {
    // a message was received
    console.log(e.data);
  };

  ws.onerror = (e) => {
    // an error occurred
    console.log(e.message);
  };

  ws.onclose = (e) => {
    // connection closed
    console.log(e.code, e.reason);
  };

  // Cleanup on Unmount
  return () => {
    ws.close();
  };
}, []);
```

## 2. Alternatives

For production apps, raw WebSockets are fragile (disconnects, reconnect logic). Libraries like **Socket.io-client** are often used instead for robustness.

## Key Takeaways

- **Cleanup**: Always close the connection in the `useEffect` return function to avoid memory leaks.
- **WSS**: Always use Secure WebSockets (`wss://`).
