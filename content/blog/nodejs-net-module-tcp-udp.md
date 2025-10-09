---
title: "Node.js: net(dành cho TCP) & dgram(cho UDP)"
date: 2025-10-09
tags: ["Node.js", "TCP", "UDP"]
categories: ["JavaScript"]
---

Node.js cung cấp:
- `net` để tạo TCP server/client,
- `dgram` cho UDP socket.

Mẹo:
- Thiết kế protocol rõ ràng (delimiters, length-prefix).
- Dùng `Buffer` đúng cách, chống split/merge packet.
- Thêm health-check & graceful shutdown (SIGINT/SIGTERM).
