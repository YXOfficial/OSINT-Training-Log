# OSINT TECHNIQUES (CHAPTER 9)
OSINT VM **Web Browsers** trên Debian (Build trắng + cấu hình Firefox cho điều tra)

> Mục tiêu của Chapter 9 là **biến browser thành công cụ thu thập chứng cứ**: cô lập phiên làm việc, giảm tracking, tải/ghi lại nội dung, và tái lập cấu hình giữa nhiều VM.

---

## 0) Cấu hình cho firefox và cài extension
<img width="1362" height="854" alt="image" src="https://github.com/user-attachments/assets/8d0375fe-e0b7-408c-8819-69c7a6058a1f" />
<img width="758" height="710" alt="image" src="https://github.com/user-attachments/assets/651f8a15-45b4-4495-abf1-31f7ea82d179" />

---

## 1) Tư duy kỹ thuật: Vì sao Chapter 9 quan trọng?
### 1.1 Browser
- Hầu hết dấu vết (cookie, local storage, fingerprinting, request headers) đều phát sinh từ browser.
- Nếu không kiểm soát browser → dễ “bẩn môi trường” (lẫn tài khoản, lẫn cookie, lẫn lịch sử) và khó giải trình.

### 1.2 Mục tiêu OPSEC tối thiểu trong VM điều tra
- **Tách biệt** VM điều tra với máy cá nhân (không login tài khoản thật).
- **Giảm rò rỉ** (telemetry/trackers) ở mức hợp lý nhưng không làm “vỡ” site.
- **Cô lập phiên**: mỗi nền tảng / mỗi persona có container riêng.

---
## 2) Bắt buộc: Build trắng Debian VM (Chapter-08.md)
---

## 3) Firefox: cấu hình nền (Settings) cho OSINT
### 3.1 Privacy & Security (tối thiểu)
- Enhanced Tracking Protection: **Strict** (hoặc Standard nếu site bị vỡ quá nhiều).
- “Delete cookies and site data when Firefox is closed”: **bật**.
  - Thêm **Exceptions** cho các site cần giữ phiên (VD: SearXNG instance).
<img width="736" height="187" alt="image" src="https://github.com/user-attachments/assets/a7745f8c-489c-4c21-8a9d-d2f27185cc6e" />

- Tắt lưu mật khẩu, tắt gợi ý/đề xuất không cần thiết (để giảm rò rỉ và nhiễu).

### 3.2 Search Engine (logic OSINT)
- Dùng metasearch (SearXNG) để query “thô”, sau đó chuyển sang Google/Bing khi cần đào sâu.

---

## 4) Add-ons

### 4.1 Nhóm “cô lập phiên & giảm rò rỉ”
- **Firefox Multi-Account Containers**
  - Mục đích: tách cookie/session theo “persona” hoặc theo nền tảng (Facebook/Instagram/X…).
<img width="1279" height="834" alt="image" src="https://github.com/user-attachments/assets/35ba2a9a-49b6-427c-b73e-86edc1b3cb00" />
<img width="1275" height="825" alt="image" src="https://github.com/user-attachments/assets/ef19be4a-ec79-4d1b-9684-dcea982a2637" />


- **uBlock Origin**
  - Mục đích: chặn trackers/malvertising, kiểm soát script theo site.
  - Kỹ thuật cốt lõi: khi site bị “vỡ”, em không tắt uBlock toàn bộ mà **mở từng script cần thiết**.
<img width="1178" height="516" alt="image" src="https://github.com/user-attachments/assets/64010996-fe75-4f44-944e-70e690de9663" />
<img width="1165" height="606" alt="image" src="https://github.com/user-attachments/assets/7033e87f-9b65-4af8-a35e-d76383343b60" />

- **NoScript** *(tùy chọn)*
  - Mục đích: chặn script “cứng” hơn uBlock, nhưng rất dễ phá trang.

### 4.2 Nhóm “thu thập chứng cứ”
- **FireShot** / **Nimbus**
  - Mục đích: chụp full page làm chứng cứ.
<img width="1280" height="6678" alt="image" src="https://github.com/user-attachments/assets/3a66f01e-2d05-4f51-a9b8-fb6e95386d28" />


- **SingleFile**
  - Mục đích: lưu cả trang thành 1 file HTML (giữ được nhiều thành phần hơn screenshot).
<img width="1270" height="766" alt="image" src="https://github.com/user-attachments/assets/e5c92810-1bc6-4bfd-8a57-779713ce89eb" />


- **Web Archives**
  - Mục đích: tra phiên bản lưu trữ (Wayback/Archive.today) của trang đang mở.

### 4.3 Nhóm “tải media / phân tích nhanh”
- **DownThemAll** / **Bulk Media Downloader**
  - Mục đích: tải hàng loạt ảnh/video/audio liên kết trong 1 trang.

- **Stream Detector**
  - Mục đích: phát hiện stream (m3u8/dash) → chuyển sang yt-dlp/ffmpeg ở Chapter 10.

- **Exif Viewer**
  - Mục đích: xem metadata nhanh.

- **User-Agent Switcher and Manager**
  - Mục đích: giả lập mobile/desktop để thấy UI khác, nội dung khác, hoặc bypass chặn đơn giản.
  <img width="844" height="295" alt="image" src="https://github.com/user-attachments/assets/07d07eb8-439e-40ac-add6-3a5e9396613b" />
  <img width="933" height="312" alt="image" src="https://github.com/user-attachments/assets/1b6ac902-79af-49ed-9840-d98e7dd7504f" />



### 4.4 Nhóm “tăng tốc quy trình”
- **Copy Selected Links**: copy toàn bộ hyperlink trên trang.
- **Image Search Options** / **Search By Image**: reverse image search 1-click.
- **OneTab**: gom tabs để đỡ loạn và giảm RAM.
  
---

## 5) Export/Import cấu hình Firefox (để tái lập VM)
- Mục tiêu: cài 1 lần, clone/backup dùng lại.
<img width="1249" height="696" alt="image" src="https://github.com/user-attachments/assets/a6d8125c-838d-45b8-8efe-eb99b107ef78" />

---

## 6) Kết luận
Sau Chapter 9, em đã:
- Có Firefox cấu hình chuẩn OSINT,
- Có add-ons phục vụ thu thập chứng cứ,
- Biết export/import profile để tái lập môi trường.
