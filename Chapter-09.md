# OSINT TECHNIQUES (CHAPTER 9)
OSINT VM **Web Browsers** trên Debian (Build trắng + cấu hình Firefox cho điều tra)

> Mục tiêu của Chapter 9 không phải “cài cho đủ”, mà là **biến browser thành công cụ thu thập chứng cứ**: cô lập phiên làm việc, giảm tracking, tải/ghi lại nội dung, và tái lập cấu hình giữa nhiều VM.

---

## 0) Deliverables
<img width="1362" height="854" alt="image" src="https://github.com/user-attachments/assets/8d0375fe-e0b7-408c-8819-69c7a6058a1f" />
<img width="758" height="710" alt="image" src="https://github.com/user-attachments/assets/651f8a15-45b4-4495-abf1-31f7ea82d179" />

- Screenshot: ví dụ **Full-page capture** (FireShot/Nimbus/SingleFile) + file được lưu.
- File xuất cấu hình Firefox (profile backup) hoặc mô tả rõ cách export/import.

---

## 1) Tư duy kỹ thuật: Vì sao Chapter 9 quan trọng?
### 1.1 Browser = “điểm chạm” chính của OSINT
- Hầu hết dấu vết (cookie, local storage, fingerprinting, request headers) đều phát sinh từ browser.
- Nếu không kiểm soát browser → dễ “bẩn môi trường” (lẫn tài khoản, lẫn cookie, lẫn lịch sử) và khó giải trình.

### 1.2 Mục tiêu OPSEC tối thiểu trong VM điều tra
- **Tách biệt** VM điều tra với máy cá nhân (không login tài khoản thật).
- **Giảm rò rỉ** (telemetry/trackers) ở mức hợp lý nhưng không làm “vỡ” site.
- **Cô lập phiên**: mỗi nền tảng / mỗi persona có container riêng.

---

## 2) Bắt buộc: Build trắng Debian VM

### 2.1 Cài Debian VM (tóm tắt đúng trọng tâm)
- ISO: Debian (stable) / Debian (testing) – ghi rõ version em dùng.
- VM spec gợi ý: 2 vCPU, 4–8GB RAM, 60GB disk, NAT network.
- Trong quá trình cài:
  - Tạo user thường (vd: `deb`) + đặt mật khẩu mạnh.
  - Cài desktop theo tài liệu môn học (GNOME nếu giáo trình dùng GNOME).
- Sau khi vào hệ thống:
  - Update: `sudo apt update && sudo apt full-upgrade -y`
  - Cài gói nền tảng: `sudo apt install -y sudo curl wget git`

### 2.2 Ghi “bài học từ lỗi”
- VM tools/guest additions (nếu có) → lỗi gì? fix ra sao?
- User không có sudo → cách thêm vào sudoers:
  - `su -`
  - `usermod -aG sudo <username>`

---

## 3) Firefox: cấu hình nền (Settings) cho OSINT
### 3.1 Privacy & Security (tối thiểu)
- Enhanced Tracking Protection: **Strict** (hoặc Standard nếu site bị vỡ quá nhiều).
- “Delete cookies and site data when Firefox is closed”: **bật**.
  - Thêm **Exceptions** cho các site cần giữ phiên (VD: SearXNG instance nếu em dùng).
- Tắt lưu mật khẩu, tắt gợi ý/đề xuất không cần thiết (để giảm rò rỉ và nhiễu).

### 3.2 Search Engine (logic OSINT)
- Dùng metasearch (SearXNG) để query “thô”, sau đó chuyển sang Google/Bing khi cần đào sâu.
- Ghi rõ: em chọn instance nào (public/self-hosted) và vì sao.

---

## 4) Add-ons: cài và giải thích “vì sao cần” (không chỉ liệt kê)
> Dưới đây là danh sách khuyến nghị theo giáo trình (ưu tiên từ quan trọng → ít quan trọng hơn).  
> Em không nhất thiết phải cài tất cả trong 1 lần, nhưng **mỗi add-on em cài phải có 3 thứ**: *Mục đích → Cấu hình tối thiểu → Cách kiểm chứng*.

### 4.1 Nhóm “cô lập phiên & giảm rò rỉ”
- **Firefox Multi-Account Containers**
  - Mục đích: tách cookie/session theo “persona” hoặc theo nền tảng (Facebook/Instagram/X…).
  - Kiểm chứng: mở 2 container đăng nhập 2 account khác nhau không bị đá nhau.

- **uBlock Origin**
  - Mục đích: chặn trackers/malvertising, kiểm soát script theo site.
  - Kỹ thuật cốt lõi: khi site bị “vỡ”, em không tắt uBlock toàn bộ mà **mở từng script cần thiết** (mô tả ngắn thao tác em làm).
  - Kiểm chứng: xem số script bị block và thử bật/tắt theo nhu cầu.

- **NoScript** *(tùy chọn – dễ làm site chết)*
  - Mục đích: chặn script “cứng” hơn uBlock, nhưng rất dễ phá trang.
  - Ghi nhận: nếu em chọn uBlock là chính, ghi rõ lý do bỏ NoScript.

### 4.2 Nhóm “thu thập chứng cứ”
- **FireShot** / **Nimbus** (chọn 1 cái làm chính)
  - Mục đích: chụp full page làm chứng cứ.
  - Kiểm chứng: chụp 1 trang dài (timeline / blog) và lưu file.

- **SingleFile**
  - Mục đích: lưu cả trang thành 1 file HTML (giữ được nhiều thành phần hơn screenshot).
  - Kiểm chứng: mở file offline vẫn đọc được nội dung chính.

- **Web Archives**
  - Mục đích: tra phiên bản lưu trữ (Wayback/Archive.today) của trang đang mở.

### 4.3 Nhóm “tải media / phân tích nhanh”
- **DownThemAll** / **Bulk Media Downloader**
  - Mục đích: tải hàng loạt ảnh/video/audio liên kết trong 1 trang.

- **Stream Detector**
  - Mục đích: phát hiện stream (m3u8/dash) → chuyển sang yt-dlp/ffmpeg ở Chapter 10.

- **Exif Viewer**
  - Mục đích: xem metadata nhanh (lưu ý: ảnh trên social thường bị strip EXIF).

- **User-Agent Switcher and Manager**
  - Mục đích: giả lập mobile/desktop để thấy UI khác, nội dung khác, hoặc bypass chặn đơn giản.

### 4.4 Nhóm “tăng tốc quy trình”
- **Copy Selected Links**: copy toàn bộ hyperlink trên trang.
- **Image Search Options** / **Search By Image**: reverse image search 1-click.
- **OneTab**: gom tabs để đỡ loạn và giảm RAM.

---

## 5) Mini workflow
Chọn 1 trang bất kỳ và làm theo:
1. Mở trang trong container “Case-01”.
2. uBlock: ghi lại số script bị block, bật/tắt 1–2 script để trang hoạt động.
3. Capture chứng cứ:
   - 1 file SingleFile **hoặc** 1 full-page screenshot.
4. Nếu có media:
   - dùng Stream Detector hoặc DownThemAll để thấy khả năng thu thập.

<img width="1266" height="619" alt="image" src="https://github.com/user-attachments/assets/437d1b1b-bd06-408d-83ee-16a8ff2be139" />


---

## 6) Export/Import cấu hình Firefox (để tái lập VM)
- Mục tiêu: cài 1 lần, clone/backup dùng lại.
- Cách làm (mô tả ở mức thao tác):
  - Vào `about:profiles` để biết profile đang dùng nằm ở đâu.
  - Nén thư mục profile (tar/zip).
  - Import vào VM mới bằng cách giải nén đúng vị trí.

---

## 7) Lỗi thường gặp & bài học
- Site “vỡ” do block script quá mạnh → dùng uBlock nâng cao thay vì tắt hết.
- Containers không tự mở đúng site → tạo rule “Always open in container…”.
- Capture trang quá dài bị lỗi → đổi công cụ (FireShot ↔ Nimbus ↔ SingleFile).

---

## 8) Kết luận
Browser là “công cụ điều tra” chứ không chỉ là “công cụ lướt web”. Sau Chapter 9, em đã:
- Có Firefox cấu hình chuẩn OSINT,
- Có add-ons phục vụ thu thập chứng cứ,
- Biết export/import profile để tái lập môi trường.
