---
title: "Java: Lập trình Multicast"
date: 2025-10-17
tags: ["Java", "Multicast"]
categories: ["Java"]
draft: false
cover: "images/posts/lap-trinh-multicast.png"
---

**Hành trình khám phá lập trình Multicast trong Java: Server và Client cơ bản!**

Chào các bạn! Hôm nay mình muốn chia sẻ một trải nghiệm mới mẻ và thú vị: học cách lập trình Multicast trong Java, một phần quan trọng của môn Lập trình Mạng. Khác với TCP thông thường, Multicast cho phép gửi dữ liệu đến nhiều máy cùng lúc, giống như phát sóng radio vậy! 

**Bắt đầu với Multicast**

Multicast là kỹ thuật gửi dữ liệu đến nhiều máy trong cùng một nhóm (group) trên mạng, dùng giao thức UDP thay vì TCP. Cũng giống như mình tưởng tượng nó như một buổi phát thanh, nơi nhiều người cùng nghe một kênh! Java hỗ trợ Multicast qua lớp MulticastSocket.

Mình học mấy bước cơ bản:
- Tạo Multicast Group: Dùng địa chỉ IP đặc biệt (thường từ 224.0.0.0 đến 239.255.255.255) để định nghĩa nhóm.
- Gửi dữ liệu: Máy phát (publisher) dùng MulticastSocket để gửi tin nhắn đến nhóm.
- Nhận dữ liệu: Máy nhận (subscriber) tham gia nhóm để nhận tin nhắn.

Bài tập đầu tiên là làm một chương trình gửi tin nhắn multicast. 
Code nhỏ để thử ngay
Dưới đây là ví dụ code cơ bản mà mình đã thử. Bạn có thể copy và chạy thử nhé!

Publisher.java (Máy phát)

```js
import java.io.*;
import java.net.*;

public class Publisher {
    public static void main(String[] args) {
        try {
            InetAddress group = InetAddress.getByName("230.0.0.0");
            int port = 12345;

            MulticastSocket socket = new MulticastSocket();
            System.out.println("Máy phát đang khởi động...");

            String message = "Chào tất cả từ máy phát!";
            byte[] buffer = message.getBytes();

            DatagramPacket packet = new DatagramPacket(buffer, buffer.length, group, port);
            socket.send(packet);

            System.out.println("Đã gửi: " + message);
            socket.close();
        } catch (IOException e) {
            System.out.println("Lỗi: " + e.getMessage());
        }
    }
}
```

Subscriber.java (Máy nhận)

```js
import java.io.*;
import java.net.*;

public class Subscriber {
    public static void main(String[] args) {
        try {
            InetAddress group = InetAddress.getByName("230.0.0.0");
            int port = 12345;

            MulticastSocket socket = new MulticastSocket(port);
            socket.joinGroup(group);
            System.out.println("Máy nhận đã tham gia nhóm, đang chờ tin nhắn...");

            byte[] buffer = new byte[1024];
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length);

            socket.receive(packet);
            String message = new String(packet.getData(), 0, packet.getLength());
            System.out.println("Nhận được: " + message);

            socket.leaveGroup(group);
            socket.close();
        } catch (IOException e) {
            System.out.println("Lỗi: " + e.getMessage());
        }
    }
}
```

Cách chạy:
- Chạy nhiều bản Subscriber.java trên các terminal/IDE khác nhau (ít nhất 2 máy nhận).
- Chạy Publisher.java trên một terminal/IDE khác.
- Xem kết quả: Tất cả máy nhận sẽ nhận được tin nhắn cùng lúc!

**Những thử thách nhỏ: Vấp ngã để lớn**

Hành trình không phải lúc nào cũng suôn sẻ. Mình gặp vài khó khăn như:
- Địa chỉ nhóm sai: Lúc đầu mình dùng IP ngoài phạm vi Multicast (ví dụ: 192.168.1.1), dẫn đến lỗi. Phải dùng 230.0.0.0 mới thành công!
- Không nhận được tin nhắn: Có lần quên gọi joinGroup(), làm máy nhận không tham gia nhóm. Thêm dòng đó là fix ngay.
- Mạng không hỗ trợ: Mình thử trên máy ảo, nhưng mạng bị chặn Multicast. Chuyển sang mạng LAN thật là ổn.

Sau khi mày mò, mình thấy mình tiến bộ:
- Hiểu cách Multicast gửi dữ liệu đến nhiều máy cùng lúc.
- Biết dùng MulticastSocket để thiết lập nhóm và truyền tin.
- Tự tin xử lý lỗi khi làm việc với UDP.

**Lời khuyên cho bạn**
- Dùng địa chỉ Multicast hợp lệ (230.0.0.0 đến 239.255.255.255).
- Chạy nhiều máy nhận trước, rồi mới chạy máy phát.