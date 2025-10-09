---
title: "JavaScript Event Loop & Network I/O"
date: 2025-10-09
tags: ["JavaScript", "EventLoop", "Async"]
categories: ["JavaScript"]
---

Event Loop là trái tim của JS runtime. Network I/O diễn ra **bất đồng bộ**:
- **Task Queue** (macro), **Microtask Queue** (Promise callbacks).
- Thực hành: tránh block main thread, dùng `async/await` và `Promise.allSettled` khi cần.

Debug tip: thêm timestamp và request-id khi log để truy lần.
