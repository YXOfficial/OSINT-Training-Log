# OSINT TECHNIQUES (CHAPTER 12)
**OSINT VM APIs: Automating Data Retrieval**

> **Mục tiêu Chapter 12:**
> Kết nối các Script đã tạo ở Chapter 11 với sức mạnh của **API (Application Programming Interface)**.
> - Hiểu tư duy dùng API thay vì duyệt web thủ công.
> - Đăng ký các dịch vụ dữ liệu (PDL, Twilio, Breach Directory...).
> - Cấu hình API Key vào script `api.sh` để truy xuất dữ liệu chỉ với 1 cú click.

---

## 1. TƯ DUY KỸ THUẬT: TẠI SAO CẦN API?

Trong Chapter 9 (Browser), em tìm kiếm thông tin bằng cách vào web -> đăng nhập -> gõ từ khóa -> chờ load -> copy kết quả.
Với **API**, em bỏ qua giao diện web. Script của em sẽ "nói chuyện" trực tiếp với server dữ liệu.

*   **Lợi ích:** Nhanh hơn, kết quả trả về dạng văn bản sạch (JSON), và quan trọng là có thể tự động hóa hàng loạt.
*   **Yêu cầu:** Cần có **API Key** (giống như mật khẩu riêng để server nhận diện ai đang hỏi).

---

## 2. THU THẬP API KEYS (DATA SOURCES)

Em tiến hành đăng ký tài khoản (Trial/Free Tier) tại các dịch vụ được Michael Bazzell giới thiệu để lấy API Key.

### 2.1. People Data Labs (PDL) - Thông tin cá nhân
*   **Mục đích:** Tìm hồ sơ con người (Email, Phone, Social Profiles) cực kỳ chi tiết.
*   **Thực hiện:**
    1.  Truy cập trang chủ PDL, đăng ký tài khoản (Free 1,000 queries).
    2.  Lấy API Key từ Dashboard.
    3.  Test thử bằng lệnh `curl` thủ công trước khi đưa vào script.

```bash
# Cấu trúc test manual (Thay KEY và EMAIL)
curl "https://api.peopledatalabs.com/v5/person/enrich?pretty=true&api_key=YOUR_PDL_KEY&email=target@example.com"
```

> **📷 YÊU CẦU CHỤP ẢNH [1]:**
> Chụp kết quả JSON trả về trong Terminal (có thể làm mờ thông tin nhạy cảm).
> *Mục đích:* Chứng minh API hoạt động và trả về dữ liệu thô.

### 2.2. Twilio / Telnyx - Caller ID
*   **Mục đích:** Truy vết số điện thoại (Carrier, Line Type, Name).
*   **Thực hiện:**
    1.  Đăng ký Twilio (cần verify số điện thoại thật).
    2.  Lấy **Account SID** và **Auth Token**.
    3.  Test manual bằng `curl` với tham số `-u SID:Token`.

```bash
# Cấu trúc test manual Twilio
curl -X GET 'https://lookups.twilio.com/v1/PhoneNumbers/+12025551212?Type=caller-name&Type=carrier' \
-u AC_YOUR_SID:YOUR_AUTH_TOKEN | python3 -m json.tool
```

### 2.3. Breach Directory (via RapidAPI) - Dữ liệu rò rỉ
*   **Mục đích:** Tìm mật khẩu cũ, hash, hoặc các trang web mà mục tiêu từng đăng ký.
*   **Thực hiện:**
    1.  Đăng ký tài khoản tại `rapidapi.com`.
    2.  Tìm "Breach Directory" và subscribe gói Free (Basic).
    3.  Lấy `X-RapidAPI-Key`.

---

## 3. CẤU HÌNH SCRIPT (TÍCH HỢP KEYS)

Đây là bước quan trọng nhất của Chapter 12: Đưa các chìa khóa vừa lấy được vào "cỗ máy" `api.sh` đã tải ở Chapter 11.

### Bước 3.1: Mở file script để chỉnh sửa
Thay vì dùng `curl` gõ tay mỗi lần, em sẽ lưu key vào script.

```bash
cd ~/Documents/scripts
# Mở file bằng Text Editor (gedit hoặc nano)
nano api.sh
```

### Bước 3.2: Thay thế Placeholder
Trong file `api.sh`, tác giả để sẵn các vị trí chờ điền key, thường ký hiệu là `XXX` hoặc `KEY=`. Em tìm đúng đoạn code của từng dịch vụ và dán key thật vào.

*Ví dụ đoạn code cần sửa cho PDL:*
```bash
# Code cũ (trong file):
# ...api_key=XXXX&email=...

# Code mới (sau khi em sửa):
# ...api_key=5c0ck097aa376bb7...&email=...
```

> **Lưu ý:**
> - Với Twilio: Cần điền cả `Account SID` và `Auth Token` (thường là chỗ `XXX:YYY`).
> - Với RapidAPI: Thay header `X-RapidAPI-Key`.

> **📷 YÊU CẦU CHỤP ẢNH [2]:**
> Chụp màn hình trình soạn thảo code (nano/gedit) đang mở file `api.sh`, hiển thị đoạn em đã dán Key (có thể che bớt một phần key để bảo mật).
> *Mục đích:* Chứng minh kỹ năng cấu hình script ("Hardcoding credentials").

---

## 4. VẬN HÀNH & KIỂM THỬ (VERIFICATION)

Sau khi lưu file `api.sh`, em sử dụng **API Tool** qua giao diện đồ họa (Zenity) đã cài ở Chapter 11.

### Bước 4.1: Chạy API Tool
1.  Bấm phím Super (Windows), gõ "API Tool" (hoặc bấm icon trên Dock).
2.  Menu hiện ra -> Chọn dịch vụ vừa cấu hình (ví dụ: "PDL Email").
3.  Nhập email mục tiêu.

### Bước 4.2: Kết quả
Script sẽ tự động:
1.  Gửi yêu cầu kèm API Key lên server.
2.  Tải kết quả về thư mục `~/Documents/API/`.
3.  Tự động mở file kết quả `.txt` hoặc `.json` lên cho em xem.

> **📷 YÊU CẦU CHỤP ẢNH [3]:**
> Chụp màn hình kết quả file text mở ra sau khi chạy tool.
> *Mục đích:* Xác nhận toàn bộ quy trình: Script -> API -> Data -> Report hoạt động trơn tru.

---

## TỔNG KẾT CHAPTER 12

Em đã hoàn thành việc nâng cấp "vũ khí" OSINT:
1.  **Hiểu bản chất:** Dữ liệu OSINT không chỉ nằm trên giao diện web mà nằm trong các database backend.
2.  **Resource:** Đã sở hữu các tài khoản truy xuất dữ liệu mạnh (PDL, RapidAPI, Twilio).
3.  **Automation:** Script `api.sh` giờ đây là trung tâm điều khiển, giúp em tra cứu thông tin định danh (Phone, Email, Breach) chỉ trong vài giây mà không cần nhớ dòng lệnh phức tạp.
