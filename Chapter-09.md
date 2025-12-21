# OSINT TECHNIQUES (CHAPTER 9)
**Môi trường:** Firefox ESR (Linux Debian)

---

## 1. CHIẾN LƯỢC TRÌNH DUYỆT (BROWSER STRATEGY)

Theo Chapter 9, trình duyệt là công cụ quan trọng nhất trong OSINT. Tác giả khuyến nghị **Firefox** thay vì Chrome/Edge do khả năng tùy biến bảo mật (Hardening) và hệ sinh thái Extension mạnh mẽ hỗ trợ điều tra.

Em đã tiến hành cấu hình Firefox trên Kali Linux như sau:

### A. Privacy Hardening (Cấu hình lõi)
- [x] **Telemetry:** Đã tắt toàn bộ "Recommend extensions/features" và "Firefox Data Collection" để chặn gửi dữ liệu hành vi về Mozilla.
- [x] **Enhanced Tracking Protection:** Thiết lập mức **Strict** (Chặn social media trackers, fingerprinting, cryptominers).
- [x] **Permissions:** Chặn vĩnh viễn request Location, Camera, Microphone để tránh bị website thu thập thông tin thiết bị.

### B. The Toolset (Các Extension bắt buộc)
Em đã cài đặt bộ Add-ons tiêu chuẩn cho môi trường điều tra:

1.  **uBlock Origin:** Chặn quảng cáo, malware domains và các script theo dõi. (Đây là lá chắn đầu tiên khi truy cập các trang web lạ).
2.  **Firefox Multi-Account Containers:**
    *   *Chức năng:* Cô lập cookie và session.
    *   *Ứng dụng:* Đăng nhập nhiều tài khoản Facebook/Gmail (Clone/Sock puppets) trên cùng một cửa sổ mà không bị phát hiện hoặc dính dây với nhau.
3.  **User-Agent Switcher and Manager:** Giả mạo thông tin thiết bị (Ví dụ: Giả làm iPhone/Android) để vượt qua các cơ chế chặn hoặc xem giao diện mobile của đối tượng.
4.  **SingleFile:** Lưu trữ toàn bộ nội dung trang web (Bao gồm ảnh, CSS) vào 1 file HTML duy nhất để làm bằng chứng Offline.
5.  **FireShot:** Chụp màn hình toàn trang (Full Page Screenshot) phục vụ báo cáo.

## 2. KẾT QUẢ
Trình duyệt đã sẵn sàng cho các tác vụ OSINT. Danh tính và Dữ liệu được bảo vệ bởi các lớp Sandbox (Container) và Blocker.

<img width="859" height="514" alt="image" src="https://github.com/user-attachments/assets/407732d4-8034-4e54-9ca5-27b6f5a352b7" />
<img width="778" height="563" alt="image" src="https://github.com/user-attachments/assets/a6b1b4af-0534-45b4-8c7b-1198fbe929aa" />
