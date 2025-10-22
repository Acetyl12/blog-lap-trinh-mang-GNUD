---
title: "Java: Lập trình đa tuyến - Multi Thread"
date: 2025-10-09
tags: ["Java", "RMI"]
categories: ["Java"]
draft: false
cover: "/images/posts/java-phan-tan-doi-tuong-rmi.png"
---

**Hành trình khám phá phân tán đối tượng trong Java bằng RMI**
Chào các bạn! Hôm nay mình muốn chia sẻ một bài học thú vị: học về phân tán đối tượng (object distribution) trong Java bằng RMI (Remote Method Invocation), một phần quan trọng trong môn Lập trình Mạng. Đây là lần đầu mình tiếp cận với cách gọi phương thức từ xa.

**RMI là gì?**
RMI là một công nghệ của Java cho phép gọi phương thức từ một đối tượng trên máy này sang máy khác, giống như "gửi lời mời" qua mạng. Nó dùng để xây dựng ứng dụng phân tán, nơi các máy phối hợp làm việc. Hãy tưởng tượng nó như một đội làm việc từ xa, mỗi người ở một nơi nhưng vẫn "hợp sức"!

Bài học bắt đầu với các khái niệm cơ bản:
- Remote Interface: Một giao diện định nghĩa các phương thức có thể gọi từ xa. Nó phải kế thừa java.rmi.Remote.
- Stub và Skeleton: Stub là "người đại diện" trên client, gửi yêu cầu đến server; Skeleton (trong phiên bản cũ) xử lý trên server. Từ Java 5, skeleton đã tự động sinh.
- Registry: Một dịch vụ tên miền (như rmiregistry) để đăng ký và tìm đối tượng từ xa.
- Quy trình cơ bản: Viết interface, triển khai server, chạy registry, rồi kết nối từ client.

Code nhỏ để thử ngay
Dưới đây là ví dụ code cơ bản mà mình đã làm. Bạn có thể copy và thử trên hai máy hoặc dùng localhost!

RemoteInterface.java (Giao diện từ xa)

```js
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface RemoteInterface extends Remote {
    int add(int a, int b) throws RemoteException;
}

ServerImpl.java (Triển khai server)
import java.rmi.registry.Registry;
import java.rmi.registry.LocatorRegistry;
import java.rmi.server.UnicastRemoteObject;

public class ServerImpl implements RemoteInterface {
    public ServerImpl() {}
    public int add(int a, int b) throws RemoteException {
        return a + b;
    }

    public static void main(String[] args) {
        try {
            ServerImpl server = new ServerImpl();
            RemoteInterface stub = (RemoteInterface) UnicastRemoteObject.exportObject(server, 0);
            Registry registry = LocateRegistry.createRegistry(1099);
            registry.bind("RemoteAdd", stub);
            System.out.println("Server đã sẵn sàng!");
        } catch (Exception e) {
            System.err.println("Lỗi server: " + e.toString());
        }
    }
}
```

Client.java (Client gọi từ xa)

```js
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class Client {
    public static void main(String[] args) {
        try {
            Registry registry = LocateRegistry.getRegistry("localhost", 1099);
            RemoteInterface stub = (RemoteInterface) registry.lookup("RemoteAdd");
            int result = stub.add(5, 3);
            System.out.println("Kết quả: 5 + 3 = " + result);
        } catch (Exception e) {
            System.err.println("Lỗi client: " + e.toString());
        }
    }
}
```

Cách chạy:
- Compile tất cả file: javac *.java.
- Chạy rmiregistry trong thư mục chứa class (trong terminal: rmiregistry).
- Chạy server: java -Djava.security.policy=policy ServerImpl.
- Chạy client: java -Djava.security.policy=policy Client.
- Xem kết quả: Client in ra "Kết quả: 5 + 3 = 8"!

Lưu ý: Tạo file policy với nội dung grant { permission java.security.AllPermission; }; để tránh lỗi bảo mật.

Sau khi mày mò RMI, mình thấy thay đổi lớn:
- Hiểu phân tán: Nắm cách gọi phương thức từ xa, xử lý serialization, và quản lý registry.
- Kỹ năng thực hành: Thành thạo cấu hình RMI, từ viết interface đến chạy client/server.
- Tư duy hệ thống: Hiểu cách xây dựng ứng dụng phân tán, mở ra đam mê với cloud computing.
- Debugging nâng cao: Khả năng tìm lỗi như ClassNotFound hay port conflict tốt hơn.
- Chuẩn bị tương lai: Đây là nền tảng cho các công nghệ như CORBA hay gRPC.

**Lời khuyên cho bạn muốn học**
- Học lý thuyết trước: Hiểu Remote Interface và serialization trước khi code.
- Thực hành nhiều: Thử lab đơn giản, rồi mở rộng (như tính toán, chat).
- Chạy đúng thứ tự: Registry -> Server -> Client, đừng nhầm!
- Sử dụng tài liệu: Đọc Java RMI Tutorial trên Oracle hoặc hỏi cộng đồng Stack Overflow.
- Kiên nhẫn: Lỗi là bình thường, cứ mò đi!
