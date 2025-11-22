# OSINT TECHNIQUES (CHAPTER 6)
**Ngày:** 22/11/2025
**Nội dung:** Tối ưu hóa Windows Host & Bảo mật dữ liệu

---

## 1. TẠI SAO CẦN "HARDENING" WINDOWS HOST?

Ở chương 6, em nhận thấy tác giả cảnh báo về việc Windows mặc định thu thập rất nhiều dữ liệu người dùng (Telemetry). Vì máy Host là nền tảng chứa các máy ảo, nó cần phải sạch và bảo mật.

Việc tối ưu hóa máy Host (Windows) tập trung vào hai mục tiêu:
1.  **Giảm thiểu Telemetry:** Chặn các kết nối không cần thiết gửi về Microsoft để bảo vệ quyền riêng tư.
2.  **Phân tách dữ liệu:** Mật khẩu và tài liệu quan trọng phải nằm ở Host, tách biệt hoàn toàn với môi trường máy ảo dễ bị lây nhiễm.

Chiến lược quan trọng nhất là **Offline Password Manager**: Lưu trữ database mật khẩu trên máy thật (Host) và chỉ copy/paste sang VM khi cần.

## 2. THỰC HÀNH: CẤU HÌNH BẢO MẬT

*   **Telemetry Control:** Sử dụng **O&O ShutUp10++** (Cấu hình Recommended) để tắt các tracker của hệ thống.
*   **System Cleaning:** Sử dụng **BleachBit** thay thế CCleaner để dọn dẹp cache an toàn.
*   **Password Vault:** Cài đặt **KeePassXC** trên Windows. Database (`.kdbx`) được lưu trữ offline, đảm bảo nếu VM bị hack thì kho mật khẩu vẫn an toàn.

<img width="1421" height="727" alt="image" src="https://github.com/user-attachments/assets/7ae00c14-4fb6-4e65-9539-e454437dbdc2" />
<img width="788" height="614" alt="image" src="https://github.com/user-attachments/assets/04435ce2-e3a2-41cf-bbf5-8837bc84f20a" />

*(Hình ảnh: Giao diện O&O ShutUp10++ đã apply settings và KeePassXC trên Windows)*

## 3. CHECKLIST

- [x] Đã chặn Telemetry bằng O&O ShutUp10++.
- [x] Hiểu và áp dụng chiến lược quản lý mật khẩu "Air-gap" (Host giữ pass, VM chỉ dùng).
- [x] Đảm bảo Windows Defender hoạt động ổn định, không cài thêm AV bên thứ 3.
