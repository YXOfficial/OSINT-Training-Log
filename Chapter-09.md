# OSINT TECHNIQUES (CHAPTER 10)
Xây dựng kho vũ khí ứng dụng trên Debian (Manual Installation)
Dưới đây là báo cáo về quá trình thiết lập các ứng dụng OSINT thủ công (trước khi sử dụng Script tự động ở chương sau).

---

## 1. TƯ DUY KỸ THUẬT (WHY MANUAL?)
Tại sao Chapter 10 bắt buộc phải cài tay (Manual) trong khi có Script?
Em nhận ra mục đích không chỉ là để có phần mềm dùng, mà là:
*   **Kiểm soát Dependencies:** Hiểu được tại sao một tool Python cũ không chạy được trên Debian 12+ (do chính sách `EXTERNALLY-MANAGED`).
*   **Khả năng giải trình:** Nếu sau này ra tòa hoặc làm báo cáo điều tra, em phải giải thích được quy trình hoạt động của công cụ ("Em đã gõ lệnh để trích xuất frame" thay vì "Em bấm nút nó tự ra").
*   **Troubleshooting:** Khi Script ở Chapter 11 bị lỗi (do repo thay đổi), em có kỹ năng để sửa từng phần nhỏ.

## 2. QUÁ TRÌNH THỰC HIỆN & BÀI HỌC

### A. Xử lý môi trường Python (Bài học lớn nhất)
Debian 12/13 chặn việc cài gói Python trực tiếp vào hệ thống (để tránh làm hỏng OS). Em đã phải xử lý bằng 2 cách:
1.  **Pipx:** Dùng để cài các tool chạy độc lập (như `yt-dlp`, `sherlock`). Nó tạo môi trường ảo riêng cho từng tool -> Rất sạch sẽ.
2.  **Can thiệp hệ thống (Warning):** Theo hướng dẫn sách (Page 89), em đã phải xóa file chặn `EXTERNALLY-MANAGED` trong `/usr/lib/python3.xx`.
    *   *Nhận định:* Đây là bước "dirty fix" nhưng cần thiết để tương thích với các hướng dẫn cũ hoặc các tool chưa hỗ trợ venv chuẩn.

### B. Các nhóm công cụ cốt lõi đã triển khai

#### 1. Video & Media (FFmpeg & YT-DLP)
*   **Cài đặt:** Kết hợp giữa `apt` (cho FFmpeg) và `pipx` (cho yt-dlp).
*   **Ứng dụng thực tế:** Không chỉ tải video, `yt-dlp` kết hợp `ffmpeg` giúp em tải được cả các video luồng (stream) m3u8 mà trình duyệt không save as được. Em đã test thử tải video Bob Ross như sách hướng dẫn để verify.

#### 2. Username & Email OSINT
*   **Công cụ:** Sherlock, Maigret.
*   **Vấn đề gặp phải:** Một số tool yêu cầu hàng tá thư viện phụ thuộc (dependencies). Việc dùng `venv` (Virtual Environment) là bắt buộc để không biến máy Debian thành bãi rác thư viện.

#### 3. Metadata & Archiving
*   **Công cụ:** Exiftool, Waybackpy.
*   **Giá trị:** Em hiểu được cách trích xuất dữ liệu ẩn (GPS, Device info) từ ảnh gốc, và cách tự động hóa việc kiểm tra lịch sử trang web mà không cần click tay trên Archive.org.

## 3. KẾT LUẬN & HƯỚNG TIẾP THEO
Việc cài đặt thủ công tốn nhiều thời gian và gặp nhiều lỗi `PATH` (biến môi trường), nhưng nó giúp em tự tin hơn khi gõ lệnh trên Terminal.

Hiện tại hệ thống Debian đã có đủ các thư viện nền tảng (Base dependencies). Em đã sẵn sàng chuyển sang **Chapter 11** để học cách viết/sử dụng Bash Script nhằm tự động hóa quy trình này cho các lần build máy sau.

*(Đính kèm: Screenshot quá trình xử lý lỗi Python Environment và kết quả chạy thử YT-DLP thành công)*
