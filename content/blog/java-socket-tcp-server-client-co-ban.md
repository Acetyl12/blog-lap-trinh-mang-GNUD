---
title: "Java Socket TCP: Server/Client cơ bản"
date: 2025-10-09
tags: ["Java", "TCP", "Socket"]
categories: ["Java"]
draft: false
cover: "images/posts/java-tcp-socket-co-ban.png"
---

Hành trình khám phá Java Socket TCP: Tạo Server và Client cơ bản!
Chào các bạn! Hôm nay mình muốn chia sẻ một trải nghiệm siêu thú vị: học cách làm một Server và Client TCP cơ bản bằng Java. Đây là một phần quan trọng trong môn Lập trình Mạng, và mình đã thử nghiệm với vài dòng code nhỏ để "nói chuyện" giữa hai máy.

**Bắt đầu với Socket TCP: Nghe lạ nhưng dễ hiểu**
TCP là giao thức truyền dữ liệu ổn định, đảm bảo thông tin đến đúng chỗ, còn socket là "cầu nối" giữa máy chủ và máy khách.
Mình bắt đầu với mấy bước cơ bản:
- Tạo Server: Dùng ServerSocket để mở cổng chờ kết nối.
- Tạo Client: Dùng Socket để kết nối đến server.
- Gửi nhận dữ liệu: Dùng luồng (InputStream, OutputStream) để truyền tin nhắn.

Bài tập đầu tiên là làm một chương trình gửi tin nhắn từ client đến server.
Dưới đây là một ví dụ code cơ bản mà mình đã làm. Bạn có thể copy và thử luôn nhé!

Server.java
```js
import java.io.*;
import java.net.*;

public class Server {
    public static void main(String[] args) {
        try {
            ServerSocket serverSocket = new ServerSocket(12345);
            System.out.println("Server đang chờ kết nối...");

            Socket socket = serverSocket.accept();
            System.out.println("Kết nối từ: " + socket.getInetAddress());

            BufferedReader reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter writer = new PrintWriter(socket.getOutputStream(), true);

            String message = reader.readLine();
            System.out.println("Nhận được: " + message);
            writer.println("Server: Nhận được tin nhắn của bạn!");

            socket.close();
            serverSocket.close();
        } catch (IOException e) {
            System.out.println("Lỗi: " + e.getMessage());
        }
    }
}
```


Client.java
```js
import java.io.*;
import java.net.*;

public class Client {
    public static void main(String[] args) {
        try {
            Socket socket = new Socket("localhost", 12345);
            System.out.println("Kết nối thành công!");

            PrintWriter writer = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            BufferedReader console = new BufferedReader(new InputStreamReader(System.in));

            System.out.print("Nhập tin nhắn: ");
            String message = console.readLine();
            writer.println(message);

            String response = reader.readLine();
            System.out.println("Phản hồi: " + response);

            socket.close();
        } catch (IOException e) {
            System.out.println("Lỗi: " + e.getMessage());
        }
    }
}
```

Cách chạy:
- Chạy Server.java trước trên một terminal/IDE.
- Chạy Client.java trên terminal/IDE khác.
- Nhập tin nhắn (ví dụ: "Chào server!"), rồi xem kết quả!


Không phải lúc nào cũng mượt mà. Mình gặp vài lỗi như:

- Kết nối lỗi: Lúc đầu mình quên mở server trước, dẫn đến client báo lỗi. Phải chạy server trước mới được.
- Dữ liệu không gửi: Có lần quên flush() trong PrintWriter, làm tin nhắn không đến. Thêm dòng true khi tạo PrintWriter là fix ngay.
- Cổng bị chiếm: Mình thử cổng 12345 nhưng bị lỗi, hóa ra có thể bị phần mềm khác dùng rồi. Đổi sang 54321 là xong.

**Cái được sau hành trình**
Sau khi mày mò, mình thấy mình tiến bộ:
- Hiểu cách TCP đảm bảo dữ liệu truyền đi an toàn.
- Biết dùng socket để tạo kết nối cơ bản.
- Tự tin debug hơn khi gặp lỗi.

Mình bắt đầu mơ về việc làm một chat nhóm nhỏ hoặc server xử lý nhiều người dùng. Hành trình này mở ra thế giới lập trình mạng cho mình!

**Lời khuyên cho bạn**
Nếu bạn muốn thử, cứ nhảy vào nhé! Mấy tip của mình:
- Chạy server trước, rồi mới client.
- Thực hành nhiều để quen với luồng dữ liệu.