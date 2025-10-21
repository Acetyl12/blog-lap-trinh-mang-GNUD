---
title: "Java NIO & Selector: Xử lý hàng nghìn kết nối trên một thread"
date: 2025-10-19
tags: ["Java", "NIO", "Selector", "Concurrency"]
categories: ["Java"]
draft: false
cover: "/images/posts/java-nio-selector-hang-nghin-ket-noi.svg"
---

![minh họa](/images/posts/java-nio-selector-hang-nghin-ket-noi.svg)

Kiến trúc non-blocking với Channel/Selector, mô hình state machine cho mỗi kết nối và tối ưu buffer.

**Từ khoá:** Java, NIO, Selector, Concurrency.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) Vì sao NIO?
Với mô hình “mỗi kết nối một thread”, chi phí context-switch + bộ nhớ tăng mạnh khi có hàng nghìn kết nối. **NIO (non-blocking I/O)** dùng `Selector` để theo dõi nhiều kênh (Channel) trên **ít thread**.

## 2) Kiến trúc
- `ServerSocketChannel` non-blocking + `Selector`
- Đăng ký sự kiện: `OP_ACCEPT`, `OP_READ`, `OP_WRITE`
- **State machine** per-connection: `READ_HEADER → READ_BODY → PROCESS → WRITE`

## 3) Phác thảo code
```java
Selector selector = Selector.open();
ServerSocketChannel ssc = ServerSocketChannel.open();
ssc.configureBlocking(false);
ssc.bind(new InetSocketAddress(8080));
ssc.register(selector, SelectionKey.OP_ACCEPT);

while (true) {
  selector.select();
  for (SelectionKey key : selector.selectedKeys()) {
    if (key.isAcceptable()) { /* accept */ }
    else if (key.isReadable()) { /* read into ByteBuffer */ }
    else if (key.isWritable()) { /* flush buffer */ }
  }
  selector.selectedKeys().clear();
}
```

## 4) Tối ưu
- Tái sử dụng `ByteBuffer` (pool).
- Gom ghi (batch write), tránh write lắt nhắt.
- Hạn chế cấp phát mới trong vòng lặp.



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
