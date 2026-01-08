# 4. HTTP Protocol Basics and Versions

## 🎯 Learning Goal

Evolution of HTTP.

## 🧠 Concept

- **HTTP/1.1**: Standard for decades. Text-based. One request per TCP connection (unless keep-alive).
- **HTTP/2**: Binary. Multiplexing (multiple requests over one connection). Server Push.
- **HTTP/3**: Based on UDP (QUIC). Faster in bad networks.

## 💻 Implementation

Most APIs today still use HTTP/1.1 or HTTP/2 semantics.

## 🧩 Activity / Challenge

1.  Open Chrome DevTools -> Network Tab.
2.  Check the "Protocol" column (h2 = HTTP/2, h3 = HTTP/3).

## 🔑 Key Takeaways

- HTTP/2 is a major performance boost for complex APIs without changing the code.
