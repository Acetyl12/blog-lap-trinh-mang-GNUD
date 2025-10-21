---
title: "Java Socket TCP cơ bản: Client/Server và giao thức thông điệp đơn giản"
date: 2025-10-19
tags: ["Java", "Networking", "TCP", "Socket"]
categories: ["Java"]
draft: false
cover: "/images/posts/java-socket-tcp-co-ban.svg"
---

![minh họa](/images/posts/java-socket-tcp-co-ban.svg)

Bắt đầu với lập trình mạng bằng Java: tạo TCP server/client, quy ước thông điệp, xử lý đóng kết nối an toàn.

**Từ khoá:** Java, Networking, TCP, Socket.  
Trong bài viết này, mình đi theo hướng *thực chiến*: giải thích ngắn gọn, kèm checklist, bẫy thường gặp và ví dụ code để bạn có thể áp dụng ngay vào dự án.



## 1) Mô hình Client/Server & giao thức
- **Server** mở port (ví dụ 8080), chấp nhận kết nối mới và tạo luồng đọc/ghi cho từng client.
- **Client** kết nối `host:port`, gửi thông điệp theo một **quy ước** (protocol) đơn giản để 2 bên hiểu nhau.
- Chọn **delimiter** (`\n`) hoặc **length-prefix** (gửi trước độ dài) để tránh dính/chia gói.

## 2) Ví dụ server tối giản (Java)
```java
import java.io.*;
import java.net.*;
public class EchoServer {
  public static void main(String[] args) throws Exception {
    try (ServerSocket ss = new ServerSocket(8080)) {
      System.out.println("Listening on 8080");
      while (true) {
        Socket s = ss.accept();
        new Thread(() -> handle(s)).start();
      }
    }
  }
  static void handle(Socket s) {
    try (s;
      BufferedReader in = new BufferedReader(new InputStreamReader(s.getInputStream()));
      PrintWriter out = new PrintWriter(s.getOutputStream(), true)) {
      String line;
      while ((line = in.readLine()) != null) {
        if ("quit".equalsIgnoreCase(line)) break;
        out.println("echo:" + line);
      }
    } catch (IOException e) { e.printStackTrace(); }
  }
}
```

## 3) Client
```java
import java.io.*;
import java.net.*;
public class EchoClient {
  public static void main(String[] args) throws Exception {
    try (Socket s = new Socket("127.0.0.1", 8080);
         BufferedReader in = new BufferedReader(new InputStreamReader(s.getInputStream()));
         PrintWriter out = new PrintWriter(s.getOutputStream(), true)) {
      out.println("hello");
      System.out.println(in.readLine());
    }
  }
}
```

## 4) Checklist an toàn
- Timeout đọc/ghi; đóng tài nguyên với `try-with-resources`.
- Ghi log có **request-id** để trace.
- Định nghĩa rõ **protocol** từ đầu; có phiên bản (v1, v2) để nâng cấp.



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
