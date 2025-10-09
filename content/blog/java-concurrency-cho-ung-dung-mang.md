---
title: "Concurrency trong ứng dụng mạng Java"
date: 2025-10-09
tags: ["Java", "Concurrency", "ThreadPool"]
categories: ["Java"]
---

3 mô hình phổ biến:
1) **Thread-per-connection** (dễ nhất, tốn tài nguyên),
2) **ThreadPool + Queue** (cân bằng hơn),
3) **NIO/Reactive** (hiệu năng cao).

Gợi ý: dùng `ExecutorService`, giới hạn pool, đo độ trễ/tải, và quản lý backpressure (hàng đợi). Với IO-bound, ưu tiên NIO/Reactor/Vert.x/Spring WebFlux.
