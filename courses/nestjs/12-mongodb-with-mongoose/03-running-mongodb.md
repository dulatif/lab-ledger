# 3. Running MongoDB

## 🎯 Learning Goal

Start a MongoDB instance.

## 🧠 Concept

We need a standard Mongo server exposing port 27017.

## 💻 Implementation

### Docker Run

```bash
docker run -d --name mongo-nest -p 27017:27017 mongo
```

### Docker Compose (Recommended)

```yaml
version: "3.8"
services:
  database:
    image: mongo
    ports:
      - "27017:27017"
```

## 🧩 Activity / Challenge

1.  Start the container.
2.  Use a GUI like **MongoDB Compass** or **TablePlus** to connect to `mongodb://localhost:27017`.
3.  It should work (and be empty).

## 🔑 Key Takeaways

- Port 27017 is the default.
- No password by default in the official image (for dev).
