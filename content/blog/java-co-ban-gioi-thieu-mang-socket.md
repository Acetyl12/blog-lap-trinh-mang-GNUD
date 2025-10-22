---
title: "Java cơ bản: Giới thiệu Lập trình Mạng & Socket"
date: 2025-10-09
tags: ["Java", "Networking", "Socket"]
categories: ["Java"]
---

Chào mừng các bạn đến với bài viết đầu tiên trong chuỗi **Java Network**! Hôm nay, mình sẽ giới thiệu tổng quan về **TCP/UDP**, vai trò của **địa chỉ IP/Port**, và mô hình **Client/Server** – những nền tảng quan trọng khi lập trình mạng bằng Java. Đây sẽ là bước khởi đầu để bạn làm quen với thế giới kết nối mạng!

## Socket là gì?
**Socket** chính là điểm đầu cuối của một kết nối mạng, nơi dữ liệu được gửi và nhận giữa các thiết bị. Trong Java, gói `java.net` cung cấp các lớp quan trọng để xử lý:
- `Socket` và `ServerSocket`: Dùng cho giao thức **TCP** (Transmission Control Protocol), đảm bảo dữ liệu được truyền đi an toàn, có thứ tự, và không mất mát.
- `DatagramSocket`: Dùng cho giao thức **UDP** (User Datagram Protocol), tập trung vào tốc độ, chấp nhận mất gói tin trong một số trường hợp.


## Khi nào dùng TCP? Khi nào dùng UDP?
- **TCP**: Chọn TCP khi bạn cần độ tin cậy cao, ví dụ như truyền file, email, hoặc ứng dụng yêu cầu dữ liệu đến đúng thứ tự (như chat). Nó giống như gửi thư được kiểm tra kỹ trước khi giao!
- **UDP**: Dùng UDP khi tốc độ là ưu tiên hàng đầu và chấp nhận mất mát, như trong streaming video, gaming online, hoặc phát thanh đa điểm (multicast). Nó nhanh như ném bóng, nhưng có thể không đến đích!

## Tổng quan TCP/UDP, IP/Port
- **TCP/UDP**: TCP tạo kết nối trước khi gửi dữ liệu (handshake), trong khi UDP gửi ngay lập tức. TCP nặng hơn nhưng an toàn; UDP nhẹ nhưng không đảm bảo.
- **Địa chỉ IP**: Là "địa chỉ nhà" của máy trên mạng (ví dụ: 192.168.1.1), giúp định vị thiết bị.
- **Port**: Là "cửa sổ" cụ thể trên máy (từ 0-65535), ví dụ port 80 cho HTTP, 54321 cho ứng dụng tùy chỉnh.
- **Mô hình Client/Server**: Client gửi yêu cầu, Server phản hồi. Socket giúp hai bên "nói chuyện" qua mạng.

## Mẹo học lập trình mạng
- **Phác thảo luồng dữ liệu**: Vẽ sơ đồ từ client → server, ghi rõ từng bước (kết nối, gửi, nhận) để dễ hình dung.
- **Log mọi trạng thái**: Dùng System.out.println hoặc log framework (như SLF4J) để theo dõi lỗi, ví dụ in ra khi socket mở/đóng hoặc dữ liệu gửi đi.
- **Thực hành**: Thử code đơn giản với Socket và DatagramSocket để hiểu sự khác biệt giữa TCP và UDP.
