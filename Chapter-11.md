# OSINT TECHNIQUES (CHAPTER 11)

## 1. TỰ ĐỘNG HÓA QUY TRÌNH (AUTOMATION)

Chapter 11 cung cấp bộ **Bash Scripts** tuỳ chỉnh giúp chuyển đổi các câu lệnh Terminal phức tạp (CLI) thành giao diện Menu đơn giản (GUI) sử dụng `zenity`.

Em đã tải và cấu hình thành công các script cốt lõi vào Kali Linux:

### A. Các Script đã triển khai:
- [x] **video.sh:** Tự động hóa `yt-dlp` và `ffmpeg` (Tải video, tách nhạc, convert định dạng chỉ bằng 1 click).
- [x] **user.sh:** Menu tổng hợp để chạy `sherlock`, `holehe`, `ghunt` mà không cần nhớ cú pháp lệnh.
- [x] **image.sh:** Hỗ trợ `exiftool` và các tool phân tích ảnh.
- [x] **metadata.sh:** Trích xuất thông tin ẩn từ file.

### B. Tùy chỉnh cho Kali:
- Đã cài đặt thêm gói `zenity` để hiển thị giao diện menu.
- Đã cấp quyền thực thi (`chmod +x`) cho toàn bộ script.
- Đã test thử `video.sh` hoạt động tốt trên môi trường Kali.
<img width="1404" height="726" alt="image" src="https://github.com/user-attachments/assets/75de2b73-e25f-42e8-8b90-d35297e583c2" />
