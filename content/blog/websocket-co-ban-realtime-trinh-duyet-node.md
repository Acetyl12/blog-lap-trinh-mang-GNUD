---
title: "WebSocket cơ bản: realtime trên trình duyệt & Node"
date: 2025-10-19
tags: ["JavaScript", "WebSocket", "Realtime"]
categories: ["JavaScript"]
draft: false
cover: "/images/posts/websocket-co-ban-realtime-trinh-duyet-node.svg"
---

![minh họa](/images/posts/websocket-co-ban-realtime-trinh-duyet-node.svg)

Thiết lập kết nối 2 chiều, xác thực JWT, ping/pong, reconnect và phân kênh (rooms).

**Từ khoá:** JavaScript, WebSocket, Realtime.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) Trình duyệt
```js
const ws = new WebSocket('wss://example.com/ws?token=JWT');
ws.onopen = () => ws.send('hello');
ws.onmessage = (ev) => console.log(ev.data);
```

## 2) Node server (ws)
```js
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8081 });
wss.on('connection', (sock) => {
  sock.on('message', m => sock.send('echo:'+m));
  // ping/pong
  const ping = setInterval(() => { sock.ping(); }, 30000);
  sock.on('close', () => clearInterval(ping));
});
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
