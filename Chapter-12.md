# OSINT TECHNIQUES (CHAPTER 12)
**Trạng thái:** ĐÃ CẤU HÌNH SCRIPT / CHỜ API KEYS

## 1. CHIẾN LƯỢC API (API INTEGRATION)

Chapter 12 tập trung vào việc sử dụng các cổng kết nối (API) để truy xuất dữ liệu thô từ các dịch vụ trả phí/miễn phí mà không cần giao diện web.

Em đã chuẩn bị sẵn sàng script `api.sh` (đã tải ở Chapter 11) để tích hợp các dịch vụ sau:

### A. Các dịch vụ mục tiêu (Target Services)
Em đã lập danh sách các dịch vụ cần đăng ký tài khoản để lấy Key:
- [x] **People Data Labs:** Tra cứu thông tin cá nhân (Email, Phone, Work).
- [x] **Twilio / Telnyx:** Tra cứu Caller ID, định danh số điện thoại (CNAM).
- [x] **Breach Directory:** Tra cứu dữ liệu rò rỉ mật khẩu.
- [x] **WhoisXMLAPI / Whoxy:** Lịch sử tên miền và Whois.
- [x] **RapidAPI:** Cổng trung gian cho các tool tra cứu số điện thoại khác.

## 2. HÀNH ĐỘNG TIẾP THEO
- Script `api.sh` đã nằm trong thư mục `~/Documents/scripts/`.
- Nhiệm vụ (Pending): Đăng ký tài khoản Developer tại các trang trên -> Lấy API Key -> Paste vào file `api.sh`.
