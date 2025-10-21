---
title: "Kiểm thử code mạng Java với JUnit & WireMock"
date: 2025-10-19
tags: ["Java", "Testing", "WireMock", "JUnit"]
categories: ["Java"]
draft: false
cover: "/images/posts/kiem-thu-network-java-junit-wiremock.svg"
---

![minh họa](/images/posts/kiem-thu-network-java-junit-wiremock.svg)

Mock server HTTP, giả lập tình huống lỗi, timeouts, và viết test đáng tin cậy cho logic mạng.

**Từ khoá:** Java, Testing, WireMock, JUnit.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) Vì sao cần mock?
Kiểm thử code mạng **không phụ thuộc** hệ thống bên ngoài. `WireMock` giúp giả lập API với scenario, delay, lỗi…

## 2) Ví dụ test
```java
@Test
void should_handle_500_with_retry() {
  WireMockServer wm = new WireMockServer(8089);
  wm.start();
  wm.stubFor(get("/users").willReturn(aResponse().withStatus(500)));
  // Gọi hàm client của bạn (có retry) rồi assert hành vi
  wm.stop();
}
```



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
