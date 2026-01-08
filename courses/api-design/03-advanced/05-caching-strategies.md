# 5. Caching Strategies

## 🎯 Learning Goal

The fastest request is the one you don't serve.

## 🧠 Concept

1.  **Client Cache**: Browser.
2.  **CDN (Edge) Cache**: Cloudflare. Near user.
3.  **Gateway/Reverse Proxy Cache**: Varnish/Nginx.
4.  **Application Cache**: Redis.

## 💻 Implementation

`Cache-Control: public, max-age=3600, s-maxage=86400`.
`Vary: Accept-Encoding` (Cache gzip vs raw separately).

## 🧩 Activity / Challenge

1.  Cache Invalidation is one of the 2 hard things in CS.
2.  Use short TTLs (Time To Live) or Versioned URLs.

## 🔑 Key Takeaways

- Cache Read-Heavy data aggressively.
