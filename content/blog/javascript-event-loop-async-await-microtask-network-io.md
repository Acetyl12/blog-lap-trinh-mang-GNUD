---
title: "JavaScript Event Loop: hiểu đúng async/await, microtask & network I/O"
date: 2025-10-19
tags: ["JavaScript", "Event Loop", "Async"]
categories: ["JavaScript"]
draft: false
cover: "/images/posts/javascript-event-loop-async-await-microtask-network-io.svg"
---

![minh họa](/images/posts/javascript-event-loop-async-await-microtask-network-io.svg)

Giải thích cơ chế Event Loop, macro/microtask, cách viết async/await đúng và tránh nghẽn UI.

**Từ khoá:** JavaScript, Event Loop, Async.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) Macro vs Microtask
- **Macro**: setTimeout, setInterval, I/O.
- **Micro**: Promise callbacks, queue ưu tiên cao hơn.

## 2) async/await đúng cách
- Tránh `await` tuần tự trong vòng lặp nếu có thể `Promise.all`.
- Bọc lỗi bằng try/catch hoặc `.catch()`.

```js
async function load() {
  const [u, p] = await Promise.all([fetch('/users'), fetch('/posts')]);
  // ...
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
