# OSINT TECHNIQUES (CHAPTER 10)
Tối ưu hóa cho Kali

## 1. KHO VŨ KHÍ

Chapter 10 giới thiệu hơn 40 công cụ OSINT. Thay vì cài đặt thủ công từng tool trên Debian (dễ lỗi), em đã tận dụng kho repository của Kali Linux và `pipx` để thiết lập môi trường ổn định.

### A. Media Intelligence (Xử lý Video/Ảnh)
- [x] **FFmpeg & VLC:** Đã có sẵn (Native). Dùng để phân tích frame video và xem bằng chứng.
- [x] **YT-DLP:** Đã cài qua `pipx`. Công cụ tải video mạnh nhất hiện nay (Hỗ trợ YouTube, Facebook, Twitter...).
- [x] **ExifTool:** Đã có sẵn. Dùng để xem metadata ẩn (GPS, Device Model) trong ảnh.

### B. Person Investigation (Điều tra con người)
- [x] **Sherlock:** Đã cài đặt. Tool truy vết Username trên hàng trăm mạng xã hội.
- [x] **Holehe:** Đã cài đặt. Kiểm tra xem Email đã đăng ký những dịch vụ nào (Không gửi email cho mục tiêu -> An toàn).
- [x] **GHunt:** Đã cài đặt. Trích xuất dữ liệu từ tài khoản Google (Maps reviews, Photos, Calendar).

### C. Domain & Recon Frameworks
- [x] **Recon-ng:** Đã có sẵn. Framework trinh sát web toàn diện.
- [x] **SpiderFoot:** Đã cài đặt. Công cụ tự động hóa thu thập dữ liệu (Footprinting).
- [x] **TheHarvester:** Đã có sẵn. Thu thập email, subdomain từ các nguồn công khai.

## 2. GHI CHÚ KỸ THUẬT
- Đã chuyển đổi sang sử dụng **`pipx`** (Python Isolated Environments) theo khuyến nghị của sách để tránh xung đột thư viện Python trên Kali.
- Các tool framework lớn (Recon-ng, Spiderfoot) được cài qua `apt` để đảm bảo tính ổn định và cập nhật từ Kali Dev Team.
