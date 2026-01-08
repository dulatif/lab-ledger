# 3. Anatomy of an HTTP Request

## 🎯 Learning Goal

Dissect a request URL.

## 🧠 Concept

`https://api.shop.com/v1/products/123?currency=USD`

- **Protocol**: `https`
- **Host**: `api.shop.com`
- **Path**: `/v1/products/123` (Hierarchical resource locator)
- **Query Parameters**: `?currency=USD` (Filtering, sorting, modification)
- **Body**: JSON data (usually for POST/PUT).

## 💻 Implementation

In a browser, the address bar is just building a GET request.

## 🧩 Activity / Challenge

1.  Go to `https://www.google.com/search?q=api`.
2.  Identify the Query Parameter (`q=api`).
3.  Change it to `q=rest` and see the result change.

## 🔑 Key Takeaways

- Path identifies the _Thing_ (Resource).
- Query modifies the _View_ of the thing.
