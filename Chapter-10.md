# OSINT TECHNIQUES (CHAPTER 10)
OSINT VM **Applications** trên Debian (Manual Installation)

> Mục tiêu của Chapter 10 là **xây dựng kho vũ khí OSINT trên Debian trắng** theo đúng logic sách:
> - Browser (Chapter 9) dùng để **phát hiện lead/URL** (vd: m3u8/dash qua Stream Detector).
> - Terminal (Chapter 10) dùng để **tải, lưu trữ, phân tích, và trích xuất metadata** có thể giải trình được.

---

## 1) TƯ DUY KỸ THUẬT (WHY MANUAL?)
Tại sao phải cài tay thay vì “có sẵn là xong”?
- **Hiểu Dependencies & môi trường:** tool Python trên Debian mới dễ dính chính sách `EXTERNALLY-MANAGED` → bắt buộc hiểu pipx/venv và PATH.
- **Giải trình được quy trình:** báo cáo OSINT không chỉ là “em thấy”, mà là **em đã làm gì** (lệnh gì → output gì → file nào được tạo).
- **Troubleshooting thực chiến:** tool OSINT hay “gãy” do site đổi giao diện/anti-bot → cài tay giúp em biết lỗi nằm ở đâu (thiếu lib? PATH? cookie? session?).

---

## 2) QUÁ TRÌNH THỰC HIỆN & BÀI HỌC
### A. Chuẩn hóa môi trường Python (bài học lớn nhất)
> **Lý do trong OSINT:** tool nhiều, dependency lộn xộn → phải “cách ly” để VM không thành bãi rác.

1. **pipx (ưu tiên):**
   - Cài tool Python theo kiểu “mỗi tool một môi trường ảo riêng” → sạch và dễ gỡ.
2. **venv (khi buộc phải git clone):**
   - Một số tool cài từ GitHub + requirements → dùng venv để không đụng Python hệ thống.
3. **PATH là lỗi phổ biến nhất:**
   - Tool cài xong nhưng “command not found” do `~/.local/bin` chưa vào PATH.

**Em đã làm được:**
- `pipx install yt-dlp && pipx ensurepath` ✅  
- (Em còn dùng `sudo rm EXTERNALLY-MANAGED` theo hướng dẫn của pdf)  
  - *Nhận định:* Đây là “dirty fix” chỉ nên dùng trong VM lab để theo kịp pdf; hướng sạch vẫn là pipx/venv.

**Verify**
```bash
which yt-dlp
yt-dlp --version
pipx list
````
<img width="351" height="68" alt="image" src="https://github.com/user-attachments/assets/0e904bd9-9b2e-4d11-a088-fe13f2ee25c1" />

<img width="607" height="134" alt="image" src="https://github.com/user-attachments/assets/d14250cd-6369-44bc-ad78-7330cb3c4507" />


---

## 3) NHÓM VIDEO & MEDIA

### 3.1 FFmpeg (apt)

* **Vai trò trong OSINT:** xử lý video chứng cứ (convert, cắt ghép, trích frame, chuẩn hóa định dạng để nộp báo cáo).
* **Vì sao cần cho OSINT sau này:**

  * Nguồn video có thể nhiều định dạng lạ → cần convert sang mp4 để xem/đính kèm dễ.
  * Khi cần “chứng minh nội dung”, trích frame là cách đóng băng các khoảnh khắc quan trọng.
* **Cài đặt:**

```bash
sudo apt update
sudo apt install ffmpeg -y
ffmpeg -version
```
<img width="713" height="56" alt="image" src="https://github.com/user-attachments/assets/c4b66f16-758a-46aa-b3c9-e3409e99f3c3" />

* **Mini demo (ý tưởng đúng sách):**

  * Convert 1 file video bất kỳ: `ffmpeg -i input.mpg output.mp4`
  * Trích frame (tùy tình huống): trích 1 frame/giây hoặc theo mốc thời gian
    *(TODO: screenshot `ffmpeg -version` + 1 file output sau convert/trích frame)*

### 3.2 YT-DLP (pipx) ✅

* **Vai trò trong OSINT:** tải video/playlist/stream để lưu chứng cứ offline, có thể kèm subtitle/description/comments.
* **Vì sao cần cho OSINT sau này:**

  * Nhiều nội dung có thể bị xóa/ẩn → tải về là cách “đóng băng” chứng cứ.
  * Có thể cần **subtitle/description/comments** để phân tích ngữ cảnh và mốc thời gian.
* **Cài đặt (em đã làm):**

```bash
pipx install yt-dlp && pipx ensurepath
yt-dlp --version
```

* **Mini demo theo sách (YouTube test):**

```bash
cd ~/Desktop
yt-dlp "https://www.youtube.com/watch?v=lLWEXRAnQd0"
```

* **Thu thập “phần phụ trợ” (đúng tinh thần PDF):**

```bash
yt-dlp "https://www.youtube.com/watch?v=lLWEXRAnQd0" --write-description
yt-dlp "https://www.youtube.com/watch?v=lLWEXRAnQd0" --write-subs
yt-dlp "https://www.youtube.com/watch?v=lLWEXRAnQd0" --write-comments
```

* **Móc nối Chapter 9 → 10 (stream m3u8/dash):**

  * Dùng Stream Detector lấy URL m3u8 → đưa URL đó vào yt-dlp để tải.
    *(TODO: screenshot tải thành công + file output / hoặc log terminal)*

### 3.3 Streamlink (pipx hoặc venv)

* **Vai trò trong OSINT:** xử lý stream/live (trường hợp yt-dlp không ổn hoặc cần ghi stream).
* **Vì sao cần cho OSINT sau này:**

  * Nguồn live có thể không xem lại được → phải ghi/tải tại thời điểm xảy ra.
  * Là “tool dự phòng” khi nguồn stream thay đổi.
* **Cài đặt (ưu tiên pipx):**

```bash
pipx install streamlink
pipx ensurepath
streamlink --version
```

* **Mini demo (đúng kiểu sách):**

```bash
streamlink https://www.twitch.tv/<channel> best -o evidence.mp4
```

*(TODO: screenshot `streamlink --version` + log ghi file)*

---

## 4) NHÓM USERNAME OSINT (CHAPTER 10: USERNAMES)

### 4.1 Sherlock

* **Vai trò trong OSINT:** tìm dấu vết username trên nhiều nền tảng (mở rộng lead).
* **Vì sao cần cho OSINT sau này:**

  * Username thường bị reuse → có thể lần ra profile khác, bài viết cũ, hoặc liên kết sang nền tảng khác.
  * Dùng ở bước “scouting nhanh” trước khi đào sâu bằng tay.
* **Cài đặt:**

```bash
pipx install sherlock-project
sherlock --help
```

* **Mini demo:**

```bash
sherlock <username_can_test>
```

*(TODO: screenshot output kết quả “found/not found”)*

### 4.2 SocialScan

* **Vai trò trong OSINT:** check username/email nhanh trên nhiều dịch vụ (lọc lead).
* **Vì sao cần cho OSINT sau này:**

  * Khi có email/username, SocialScan giúp check nhanh dấu hiệu tồn tại tài khoản.
  * Dùng như bước “triage” (lọc nhanh) trước khi đi sâu.
* **Cài đặt:**

```bash
pipx install socialscan
socialscan --help
```

*(TODO: screenshot `socialscan --help` hoặc 1 lần chạy test)*

---

## 5) NHÓM EMAIL OSINT (CHAPTER 10: EMAIL)

### 5.1 Holehe

* **Vai trò trong OSINT:** kiểm tra email có đăng ký ở những dịch vụ nào (lead rất mạnh).
* **Vì sao cần cho OSINT sau này:**

  * Từ 1 email có thể suy ra nền tảng mục tiêu dùng → định hướng điều tra (social, shopping, dev, forum...).
* **Cài đặt:**

```bash
pipx install holehe
holehe --help
```

*(TODO: screenshot `holehe --help` + (nếu được) 1 lần chạy test với email demo/được phép)*

### 5.2 GHunt

* **Vai trò trong OSINT:** điều tra hệ sinh thái Google (thường cần cookie/session).
* **Vì sao cần cho OSINT sau này:**

  * Google account liên quan nhiều dịch vụ (YouTube, Drive, Maps...) → có thể mở rộng dấu vết.
* **Cài đặt:**

```bash
pipx install ghunt
ghunt --help
```

* *Ghi chú đúng thực tế:* tool này dễ fail nếu không có cookie/session hợp lệ → phải note rõ “điều kiện chạy”.
  *(TODO: screenshot cài đặt + note lỗi nếu gặp)*

### 5.3 H8mail (+ hash tools)

* **Vai trò trong OSINT:** tra breach/credential (tùy cấu hình) và nhận diện hash.
* **Vì sao cần cho OSINT sau này:**

  * Khi có leak, dữ liệu thường ở dạng hash → cần nhận diện đúng loại hash để xử lý đúng hướng.
* **Cài đặt:**

```bash
pipx install h8mail
pipx install name-that-hash
pipx install search-that-hash
```

* **Mini demo nhận diện hash (đúng tinh thần sách):**

```bash
nth --text "5f4dcc3b5aa765d61d8327deb882cf99"
sth --text "5f4dcc3b5aa765d61d8327deb882cf99"
```

*(TODO: screenshot output nhận diện MD5)*

---

## 6) NHÓM IMAGES / SOCIAL ARCHIVING (CHAPTER 10: IMAGES)

### 6.1 Gallery-dl

* **Vai trò trong OSINT:** tải hàng loạt ảnh/album để giữ chứng cứ + phục vụ reverse image search.
* **Vì sao cần cho OSINT sau này:**

  * Album có thể bị xóa/ẩn → tải về để “đóng băng”.
  * Tạo dataset ảnh để so sánh, tìm ảnh gốc, phát hiện tái sử dụng.
* **Cài đặt:**

```bash
pipx install gallery-dl
gallery-dl --help
```

*(TODO: screenshot `gallery-dl --help` hoặc 1 lần tải album public)*

### 6.2 Instagram tools (Instaloader / Toutatis / Osintgram)

* **Vai trò trong OSINT:** thu thập dữ liệu Instagram (nhưng dễ bị chặn → cần nhiều tool dự phòng).
* **Vì sao cần cho OSINT sau này:**

  * Instagram thay đổi liên tục + anti-bot → 1 tool thường không đủ, phải có phương án thay thế.
  * Session/cookie từ Firefox (Chapter 9) đôi khi là chìa khóa để chạy được.
* **Cài đặt cơ bản:**

```bash
pipx install instaloader
pipx install toutatis
```

* **Ghi chú:** Toutatis thường cần `sessionid` lấy từ cookie trình duyệt.
  *(TODO: screenshot cài đặt + note điều kiện chạy / lỗi gặp)*

---

## 7) NHÓM DOMAINS & ARCHIVING (CHAPTER 10: DOMAINS)

### 7.1 EyeWitness

* **Vai trò trong OSINT:** chụp nhanh hàng loạt website và tạo report (đóng băng trạng thái trang).
* **Vì sao cần cho OSINT sau này:**

  * Khi điều tra nhiều domain/subdomain, EyeWitness giúp “quét mặt tiền” rất nhanh.
  * Report HTML dễ đưa vào hồ sơ chứng cứ.
* **Cài đặt (git clone theo sách):**

```bash
cd ~/Downloads/Programs
git clone https://github.com/ChrisTruncer/EyeWitness.git
```

*(TODO: screenshot thư mục clone + lần chạy đầu tiên / hoặc note yêu cầu thêm dependencies)*

### 7.2 HTTrack

* **Vai trò trong OSINT:** mirror website (nhất là site tĩnh) để xem offline.
* **Vì sao cần cho OSINT sau này:**

  * Khi trang thay đổi/xóa, bản mirror giúp giữ bằng chứng (tùy trường hợp).
* **Cài đặt:**

```bash
sudo apt update
sudo apt install httrack webhttrack -y
```

*(TODO: screenshot cài đặt thành công / mở webhttrack nếu có)*

### 7.3 Waybackpy + Waybackpack

* **Vai trò trong OSINT:** tự động hóa việc lấy dữ liệu từ Wayback Machine (URL đã lưu, bản cũ nhất, tải snapshot).
* **Vì sao cần cho OSINT sau này:**

  * Đối chiếu “trước đây vs hiện tại” để dựng timeline.
  * Tự động hóa thay vì click tay trên web archive.
* **Cài đặt:**

```bash
pipx install waybackpy
pipx install waybackpack
```

*(TODO: screenshot `--help` hoặc 1 lần chạy lấy known_urls)*

### 7.4 ChangeDetection.io / ArchiveBox (tùy chọn nâng cao)

* **Vai trò trong OSINT:** theo dõi thay đổi & archive nhiều kiểu (HTML/screenshot/media…).
* **Vì sao cần cho OSINT sau này:**

  * Khi cần “canh” trang thay đổi (giá, nội dung bài viết, thông báo…).
  * ArchiveBox giúp lưu tập trung thay vì rải rác screenshot.
* **Cài đặt (khi em làm tới):**

```bash
pipx install changedetection.io
pipx install archivebox
```

*(TODO: note port/server nếu em chạy local service)*

---

## 8) NHÓM METADATA (CHAPTER 10: METADATA)

### 8.1 Exiftool

* **Vai trò trong OSINT:** đọc EXIF/GPS/device info từ ảnh gốc + export báo cáo.
* **Vì sao cần cho OSINT sau này:**

  * Nếu có ảnh gốc, EXIF có thể cung cấp thời gian/chủng loại thiết bị/vị trí → cực mạnh cho dựng timeline.
  * Có thể xuất CSV để xử lý hàng loạt (lọc theo model/time/GPS).
* **Cài đặt:**

```bash
sudo apt update
sudo apt install libimage-exiftool-perl -y
exiftool -ver
```

*(TODO: screenshot exiftool chạy trên 1 ảnh gốc hoặc file mẫu)*

### 8.2 mat2

* **Vai trò trong OSINT/OPSEC:** xóa metadata khỏi file mình chuẩn bị chia sẻ (tránh tự lộ dấu).
* **Vì sao cần cho OSINT sau này:**

  * Điều tra xong có thể phải gửi file/report → mat2 giúp tránh vô tình leak username/hostname/location.
* **Cài đặt:**

```bash
sudo apt install mat2 -y
mat2 --help
```

*(TODO: screenshot `mat2 --help` hoặc demo trên 1 file test)*

### 8.3 xeuledoc

* **Vai trò trong OSINT:** trích metadata từ Google Docs/Sheets (tùy tài liệu public).
* **Vì sao cần cho OSINT sau này:**

  * Đôi khi tài liệu lộ “tác giả/email” hoặc thông tin tổ chức → mở lead điều tra.
* **Cài đặt:**

```bash
pipx install xeuledoc
xeuledoc --help
```

*(TODO: screenshot output khi chạy với 1 link public hợp lệ)*

### 8.4 Sherloq / MediaInfo / Metagoofil (tùy chọn theo sách)

* **Sherloq:** hỗ trợ phát hiện dấu hiệu chỉnh sửa ảnh (forensics hỗ trợ).
* **MediaInfo GUI:** xem metadata video/audio bằng giao diện.
* **Metagoofil:** tìm tài liệu theo domain và trích metadata (tên người dùng/tổ chức).
  *(TODO: khi em cài tới, mỗi tool ghi 1 đoạn: vì sao cần + lệnh cài + verify)*

---

## 9) KẾT LUẬN

Sau Chapter 10, em đã hiểu và triển khai được:

* Cách quản lý tool OSINT trên Debian theo hướng **pipx/venv** để tránh dependency hell,
* Nhóm công cụ cốt lõi cho OSINT: **Video / Username / Email / Images / Domains / Metadata**,
* Và quan trọng nhất: mỗi tool đều có thể **giải trình** (cài thế nào, verify thế nào, dùng trong tình huống nào, lỗi gì và fix ra sao).
