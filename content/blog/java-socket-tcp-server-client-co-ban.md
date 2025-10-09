---
title: "Java Socket TCP: Server/Client cơ bản"
date: 2025-10-09
tags: ["Java", "TCP", "Socket"]
categories: ["Java"]
---

Ví dụ tối giản:
- **Server**: mở `ServerSocket`, chấp nhận `Socket`, đọc/ghi qua `InputStream/OutputStream`.
- **Client**: tạo `Socket(host, port)`, gửi chuỗi, nhận phản hồi.

Các bước an toàn:
1. Dùng `try-with-resources` để đóng tài nguyên.
2. Quy ước **message protocol** (ví dụ: chuỗi JSON kết thúc bằng `\n`).
3. Thử nghiệm bằng `telnet`/`nc` để kiểm tra server.
