---
title: "Java NIO: Selector & Channel cho kết nối đồng thời"
date: 2025-10-09
tags: ["Java", "NIO", "Concurrency"]
categories: ["Java"]
---

`java.nio` hỗ trợ **non-blocking I/O** với `Selector` + `Channel`, xử lý nhiều socket trên 1 thread.  
Lợi ích: tiết kiệm tài nguyên hơn so với mỗi kết nối một thread.  
Ý tưởng chính:
- `ServerSocketChannel` non-blocking + `Selector` theo dõi events `OP_ACCEPT/OP_READ/OP_WRITE`.
- Bộ đệm `ByteBuffer` tái sử dụng để giảm GC.
- Thiết kế **state machine** cho từng connection (đọc header → đọc body → xử lý → ghi).
