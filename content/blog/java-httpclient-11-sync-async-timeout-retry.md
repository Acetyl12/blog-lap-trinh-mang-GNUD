---
title: "Java 11+ HttpClient: gọi API sync/async, timeout và retry"
date: 2025-10-19
tags: ["Java", "HTTP", "API"]
categories: ["Java"]
draft: false
cover: "/images/posts/java-httpclient-11-sync-async-timeout-retry.svg"
---

![minh họa](/images/posts/java-httpclient-11-sync-async-timeout-retry.svg)

Sử dụng HttpClient để gọi REST một cách hiện đại: cấu hình timeout, redirect, async và chiến lược retry.

**Từ khoá:** Java, HTTP, API.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) Thiết lập cơ bản
`HttpClient` (Java 11+) hỗ trợ sync/async, HTTP/2, redirect, timeout.
```java
HttpClient client = HttpClient.newBuilder()
    .connectTimeout(java.time.Duration.ofSeconds(3))
    .build();

HttpRequest req = HttpRequest.newBuilder(URI.create("https://api.example.com"))
    .timeout(java.time.Duration.ofSeconds(5))
    .header("Accept","application/json")
    .build();

HttpResponse<String> res = client.send(req, HttpResponse.BodyHandlers.ofString());
```

## 2) Async với `sendAsync`
```java
client.sendAsync(req, HttpResponse.BodyHandlers.ofString())
      .thenApply(HttpResponse::body)
      .thenAccept(System.out::println)
      .join();
```

## 3) Retry & Backoff (tự viết)
- Thử lại khi lỗi mạng tạm thời (5xx/timeout).
- Exponential backoff + jitter để tránh “đụng” máy chủ.



## Best-practices nhanh
- Ghi log có cấu trúc (JSON) + traceId.
- Timeout hợp lý cho connect/read/write; tổng timeout (deadline) khi cần.
- Chuẩn hoá lỗi cho client; đừng lộ stacktrace ra ngoài.
- Viết kiểm thử với trường hợp **lỗi** (timeout, 429, 5xx), không chỉ thành công.



## Checklist áp dụng
- [ ] Thiết kế protocol hoặc schema API rõ ràng.
- [ ] Thêm retry/backoff có điều kiện.
- [ ] Quan sát (metrics/log/trace) đầy đủ.
- [ ] Tài liệu hoá ví dụ request/response (hoặc mock contract).


## Kết luận
Những nguyên tắc ở trên đủ để bạn triển khai và mở rộng trong bối cảnh dự án thực tế. Hãy clone lại ví dụ, chạy thử, thêm log và đo đạc; mọi tối ưu đều bắt đầu từ quan sát.
