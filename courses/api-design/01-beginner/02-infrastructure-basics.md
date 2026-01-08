# 2. Infrastructure: TCP/IP and DNS Basics

## 🎯 Learning Goal

How data moves across the internet.

## 🧠 Concept

- **IP (Internet Protocol)**: The address (e.g., `192.168.1.1`).
- **TCP (Transmission Control Protocol)**: Ensures delivery reliability (handshakes).
- **DNS (Domain Name System)**: Phonebook. Maps `google.com` -> `142.250.190.46`.

## 💻 Implementation

When you call `api.example.com`:

1.  DNS Lookup: "Where is `api.example.com`?" -> IP.
2.  TCP Handshake: "Hello? Can we talk?" -> "Yes".
3.  HTTP Request: "Get me data".

## 🧩 Activity / Challenge

1.  Run `ping google.com` in your terminal to see the IP.
2.  Run `nslookup google.com` to see DNS resolution.

## 🔑 Key Takeaways

- DNS outages ("It's always DNS") break APIs because clients can't find the server.
