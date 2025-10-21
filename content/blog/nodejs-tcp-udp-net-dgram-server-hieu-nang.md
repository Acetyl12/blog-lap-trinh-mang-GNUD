---
title: "Node.js TCP/UDP: net & dgram cho server hiệu năng"
date: 2025-10-19
tags: ["Node.js", "TCP", "UDP"]
categories: ["JavaScript"]
draft: false
cover: "/images/posts/nodejs-tcp-udp-net-dgram-server-hieu-nang.svg"
---

![minh họa](/images/posts/nodejs-tcp-udp-net-dgram-server-hieu-nang.svg)

Xây dựng TCP/UDP server với net/dgram, xử lý split/merge packet và shutdown an toàn.

**Từ khoá:** Node.js, TCP, UDP.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) TCP với `net`
```js
const net = require('net');
const server = net.createServer(sock => {
  sock.on('data', buf => {
    // xử lý split/merge packet bằng delimiter hoặc length-prefix
    sock.write(Buffer.from('echo:' + buf));
  });
});
server.listen(8080);
```

## 2) UDP với `dgram`
```js
const dgram = require('dgram');
const sock = dgram.createSocket('udp4');
sock.on('message', (msg, rinfo) => {
  sock.send(msg, rinfo.port, rinfo.address);
});
sock.bind(9999);
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
