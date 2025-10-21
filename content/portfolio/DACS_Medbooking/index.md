---
title: "Đồ án cơ sở - Quản lý lịch đặt hẹn khám bệnh"
date: 2025-10-20
summary: "Hệ thống quản lý đặt lịch khám bệnh thông minh sử dụng ASP.NET Core, tích hợp chatbot AI tư vấn, phân quyền Identity và quản lý lịch hẹn – đơn thuốc – xét nghiệm."
tech: ["ASP.NET Core", "C#", "Entity Framework", "Identity", "OpenAI API"]
role: "Fullstack Developer"
links:
  - { label: "Source", url: "https://github.com/Acetyl12/DACS_QLDatKhamBenh" }
  - { label: "Docs", url: "#" }
---

## 🏥 Giới thiệu dự án

Đồ án cơ sở “**Quản lý lịch đặt hẹn khám bệnh**” là một ứng dụng web được phát triển bằng **ASP.NET Core MVC**, hỗ trợ người dùng quản lý quy trình khám chữa bệnh trực tuyến – từ đăng ký, đặt hẹn, khám bệnh, kê toa, đến xét nghiệm.  
Dự án đồng thời tích hợp **AI Chatbot** giúp bệnh nhân được **tư vấn triệu chứng sơ bộ, chọn bác sĩ hoặc chuyên khoa phù hợp**, và đặt lịch trực tiếp.

---

## 🎯 Mục tiêu

- Tin học hóa quy trình đặt lịch và khám chữa bệnh.  
- Tăng trải nghiệm người dùng thông qua **AI tư vấn và tự động hóa thông báo**.  
- Cung cấp hệ thống phân quyền rõ ràng giữa **Admin – Bác sĩ – Bệnh nhân**.  
- Quản lý lịch hẹn, đơn thuốc, và lịch xét nghiệm một cách trực quan và tập trung.

---

## ⚙️ Công nghệ sử dụng

| Thành phần | Công nghệ |
|-------------|------------|
| Backend | ASP.NET Core 8.0 (C#) |
| Frontend | Razor Pages, Bootstrap 5 |
| CSDL | Microsoft SQL Server |
| ORM | Entity Framework Core |
| Authentication | ASP.NET Identity (Role-based) |
| AI Chatbot | OpenAI API / Azure OpenAI |
| Email/Notification | MailKit, SignalR |
| IDE | Visual Studio 2022 |

---

## 👥 Các vai trò trong hệ thống

| Vai trò | Quyền hạn |
|----------|-----------|
| **Admin** | Quản lý người dùng, bác sĩ, chuyên khoa, lịch hẹn và thống kê toàn hệ thống |
| **Bác sĩ** | Xem lịch khám của mình, xác nhận/huỷ lịch, kê đơn thuốc, nhập kết quả xét nghiệm |
| **Bệnh nhân** | Đăng ký, đăng nhập, chat với AI, đặt lịch khám theo bác sĩ hoặc khoa, xem đơn thuốc, xét nghiệm |

---

## 💡 Các tính năng chính

### 🔐 1. Quản lý người dùng và phân quyền
- Đăng ký / đăng nhập qua **ASP.NET Identity**  
- Quản lý **3 role:** Admin – Doctor – Patient  
- Xác thực email, đổi mật khẩu, khôi phục tài khoản

### 🤖 2. Chatbot AI tư vấn sức khỏe
- Gợi ý chuyên khoa dựa trên triệu chứng nhập vào  
- Đặt lịch khám trực tiếp qua hội thoại  
- Sử dụng OpenAI API để xử lý ngôn ngữ tự nhiên  

### 📅 3. Đặt lịch khám bệnh
- Lọc lịch theo bác sĩ, chuyên khoa hoặc thời gian  
- Kiểm tra trùng lịch, xác nhận tự động  
- Gửi thông báo khi lịch được duyệt hoặc huỷ  

### 💊 4. Kê đơn thuốc & quản lý toa thuốc
- Bác sĩ kê toa trực tuyến sau khi khám  
- Bệnh nhân có thể tra cứu lại đơn thuốc trong hồ sơ  
- Lưu trữ và cập nhật lịch sử điều trị  

### 🧪 5. Đặt và quản lý lịch xét nghiệm
- Đăng ký loại xét nghiệm (máu, X-quang, MRI, v.v.)  
- Theo dõi kết quả xét nghiệm và bác sĩ phụ trách  
- Nhận thông báo tự động khi có kết quả  

### 🔔 6. Hệ thống thông báo & báo cáo
- Gửi email và thông báo real-time (SignalR)  
- Thống kê lịch hẹn, doanh thu, số lượt khám  
- Báo cáo theo ngày / tuần / tháng  

---

