---
title: "Java cơ bản: Giới thiệu Lập trình Mạng & Socket"
date: 2025-10-09
tags: ["Java", "Networking", "Socket"]
categories: ["Java"]
---

Bài viết mở đầu chuỗi Java Network: tổng quan TCP/UDP, địa chỉ IP/Port, và mô hình Client/Server.  
**Socket** là điểm đầu cuối của kết nối mạng. Với Java, gói `java.net` cung cấp `Socket`, `ServerSocket` (TCP) và `DatagramSocket` (UDP).  
**Khi nào dùng TCP?** Khi cần tin cậy, có thứ tự. **Khi nào dùng UDP?** Khi cần tốc độ, chấp nhận mất mát (streaming, game).

> Mẹo học: phác thảo luồng dữ liệu từ client → server và log mọi trạng thái để dễ debug.
