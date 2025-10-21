---
title: "Fetch vs Axios: kiến trúc HTTP client tái sử dụng với interceptor"
date: 2025-10-19
tags: ["JavaScript", "Axios", "HTTP"]
categories: ["JavaScript"]
draft: false
cover: "/images/posts/fetch-vs-axios-kien-truc-http-client-interceptor.svg"
---

![minh họa](/images/posts/fetch-vs-axios-kien-truc-http-client-interceptor.svg)

So sánh fetch và axios, thiết kế HTTP client chung, quản lý token, refresh và chuẩn hóa lỗi.

**Từ khoá:** JavaScript, Axios, HTTP.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) So sánh nhanh
- **fetch**: native, cần tự xử lý JSON, lỗi 4xx/5xx không throw mặc định.
- **axios**: interceptor, baseURL, timeout mặc định ngon.

## 2) HTTP client tái sử dụng (axios)
```js
import axios from 'axios';
export const http = axios.create({ baseURL: '/api', timeout: 5000 });
http.interceptors.request.use(cfg => {
  // gắn token nếu có
  return cfg;
});
http.interceptors.response.use(
  res => res,
  err => {
    // chuẩn hoá lỗi, refresh token...
    return Promise.reject(err);
  }
);
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
