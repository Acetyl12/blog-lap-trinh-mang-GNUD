---
title: "Java 11+: HttpClient hiện đại (sync/async)"
date: 2025-10-09
tags: ["Java", "HTTP", "API"]
categories: ["Java"]
---

`java.net.http.HttpClient` (Java 11+) đơn giản hoá gọi REST:
- Tạo `HttpClient`, `HttpRequest`, sau đó `send()` (sync) hoặc `sendAsync()` (CompletableFuture).
- Dễ set timeout, redirect, header; parse JSON với Jackson/Gson.

Ví dụ: GET/POST, retry (tự viết), log request/response, và best-practices (timeout, circuit-breaker bên ngoài như Resilience4j).
