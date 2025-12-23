# OSINT TECHNIQUES (CHAPTER 9)
OSINT VM **Web Browsers** trên Debian (Build trắng + cấu hình Firefox cho điều tra)

> Mục tiêu của Chapter 9 là **biến browser thành công cụ thu thập chứng cứ**: cô lập phiên làm việc, giảm tracking, tải/ghi lại nội dung, và tái lập cấu hình giữa nhiều VM.

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
> **Lý do trong OSINT:**  
> - Điều tra thường phải mở nhiều nguồn “không sạch” (quảng cáo độc hại, tracker, link mồi). Tăng privacy giúp giảm rủi ro bị theo dõi/ngược dấu.  
> - Bật xóa cookie/site data khi đóng trình duyệt giúp **mỗi phiên điều tra tách biệt**, hạn chế “bẩn môi trường” và hạn chế bị gợi ý nội dung theo lịch sử trước đó.  
> - Exceptions dùng để giữ phiên ở những công cụ phục vụ điều tra (ví dụ 1 metasearch instance, hoặc site cần login bằng tài khoản “điều tra”).

- Enhanced Tracking Protection: **Strict** (hoặc Standard nếu site bị vỡ quá nhiều).
<img width="1362" height="854" alt="image" src="https://github.com/user-attachments/assets/8d0375fe-e0b7-408c-8819-69c7a6058a1f" />

- “Delete cookies and site data when Firefox is closed”: **bật**.
  - Thêm **Exceptions** cho các site cần giữ phiên (VD: SearXNG instance).
<img width="736" height="187" alt="image" src="https://github.com/user-attachments/assets/a7745f8c-489c-4c21-8a9d-d2f27185cc6e" />

- Tắt lưu mật khẩu, tắt gợi ý/đề xuất không cần thiết (để giảm rò rỉ và nhiễu).

### 3.2 Search Engine (logic OSINT)
> **Lý do trong OSINT:**  
> - Metasearch (SearXNG) phù hợp cho giai đoạn “scouting” (tìm nhanh, ít cá nhân hóa kết quả hơn).  
> - Khi đã có từ khóa/dấu vết, chuyển sang Google/Bing để đào sâu vì các công cụ này có index mạnh và nhiều operator nâng cao.  
> - Cách làm này giúp em vừa **giảm footprint** ở bước đầu, vừa tối ưu chất lượng kết quả ở bước sau.
- Dùng metasearch (SearXNG) để query “thô”, sau đó chuyển sang Google/Bing khi cần đào sâu.

---

## 4) Add-ons

### 4.1 Nhóm “cô lập phiên & giảm rò rỉ”
- **Firefox Multi-Account Containers**
  - Mục đích: tách cookie/session theo “persona” hoặc theo nền tảng (Facebook/Instagram/X…).
  - **Vì sao cần cho OSINT sau này:** 
    - Khi điều tra nhiều mục tiêu/nhiều persona, Containers giúp em **không lẫn session** (tránh “đang điều tra A nhưng cookie lại của B”).  
    - Hạn chế site cross-track giữa các tab → giảm rò rỉ hành vi duyệt web và giảm khả năng bị “recommend” nội dung sai ngữ cảnh.  
    - Dễ tái lập quy trình: mỗi case mở 1 container riêng (Case-01, Case-02...) → log chứng cứ rõ ràng.

<img width="1279" height="834" alt="image" src="https://github.com/user-attachments/assets/35ba2a9a-49b6-427c-b73e-86edc1b3cb00" />
<img width="1275" height="825" alt="image" src="https://github.com/user-attachments/assets/ef19be4a-ec79-4d1b-9684-dcea982a2637" />


- **uBlock Origin**
  - Mục đích: chặn trackers/malvertising, kiểm soát script theo site.
  - Kỹ thuật cốt lõi: khi site bị “vỡ”, em không tắt uBlock toàn bộ mà **mở từng script cần thiết**.
  - **Vì sao cần cho OSINT sau này:**  
    - OSINT hay phải mở các trang có ads/tracker nặng → uBlock giúp giảm nguy cơ dính malvertising và giảm fingerprinting cơ bản.  
    - Khi thu thập chứng cứ, giảm script thừa giúp trang tải ổn định hơn, ít pop-up làm “bẩn” screenshot/SingleFile.  
    - Kỹ năng “mở từng script cần thiết” là để cân bằng: **không tắt bảo vệ**, nhưng vẫn làm site hoạt động phục vụ thu thập dữ liệu.

<img width="1178" height="516" alt="image" src="https://github.com/user-attachments/assets/64010996-fe75-4f44-944e-70e690de9663" />
<img width="1165" height="606" alt="image" src="https://github.com/user-attachments/assets/7033e87f-9b65-4af8-a35e-d76383343b60" />

- **NoScript** *(tùy chọn)*
  - Mục đích: chặn script “cứng” hơn uBlock, nhưng rất dễ phá trang.
  - **Vì sao chỉ là tùy chọn trong OSINT:**  
    - NoScript chặn mạnh → tăng an toàn khi mở nguồn lạ, nhưng rất dễ làm website “vỡ”, gây mất dữ liệu cần thu thập.  
    - Thực tế điều tra thường ưu tiên uBlock (mềm hơn). NoScript chỉ dùng khi em cần “đi vào vùng rủi ro cao” (site mờ ám, nhiều script lạ).

### 4.2 Nhóm “thu thập chứng cứ”
- **FireShot** / **Nimbus**
  - Mục đích: chụp full page làm chứng cứ.
  - **Vì sao cần cho OSINT sau này:**  
    - Screenshot full page là dạng chứng cứ trực quan, dễ đưa vào báo cáo.  
    - Nhiều nội dung web (bài viết/timeline) rất dài → chụp từng đoạn dễ thiếu/khó đối chiếu; full page giúp giữ **tính toàn vẹn ngữ cảnh**.  
    - Khi đối tượng xóa/sửa nội dung, screenshot full page là bằng chứng nhanh nhất để “đóng băng hiện trạng”.

<img width="1280" height="1280" alt="image" src="https://github.com/user-attachments/assets/3a66f01e-2d05-4f51-a9b8-fb6e95386d28" />


- **SingleFile**
  - Mục đích: lưu cả trang thành 1 file HTML (giữ được nhiều thành phần hơn screenshot).
  - **Vì sao cần cho OSINT sau này:**  
    - SingleFile lưu trang thành 1 HTML offline → giữ được nhiều thành phần hơn screenshot (text, layout, một phần asset).  
    - Hữu ích khi em cần **trích dẫn nguyên văn** trong báo cáo, hoặc cần mở lại đúng nội dung mà không phụ thuộc website còn tồn tại.  
    - So với bookmark/link: file HTML là “bản chụp” tại thời điểm thu thập, có giá trị chứng cứ cao hơn.

<img width="1270" height="766" alt="image" src="https://github.com/user-attachments/assets/e5c92810-1bc6-4bfd-8a57-779713ce89eb" />


- **Web Archives**
  - Mục đích: tra phiên bản lưu trữ (Wayback/Archive.today) của trang đang mở.
  - **Vì sao cần cho OSINT sau này:**  
    - Điều tra thường gặp trang bị xóa hoặc sửa nội dung → tra Wayback/Archive.today giúp phục hồi “phiên bản cũ”.  
    - Dùng để đối chiếu: “trước đây họ ghi gì” vs “bây giờ họ ghi gì” (hữu ích khi viết timeline).  
    - Khi báo cáo, có thể dẫn nguồn từ bản lưu trữ để tăng tính kiểm chứng.

### 4.3 Nhóm “tải media / phân tích nhanh”
- **DownThemAll** / **Bulk Media Downloader**
  - Mục đích: tải hàng loạt ảnh/video/audio liên kết trong 1 trang.
  - **Vì sao cần cho OSINT sau này:**  
    - Khi mục tiêu có album/loạt ảnh dài, tải thủ công vừa chậm vừa dễ sót.  
    - Tải hàng loạt giúp em nhanh chóng có dataset để: reverse image search, so sánh ảnh, tìm ảnh gốc, hoặc lọc ảnh theo thời gian.  
    - Giảm thao tác click → giảm footprint và giảm nguy cơ bị rate-limit do hành vi lặp.

- **Stream Detector**
  - Mục đích: phát hiện stream (m3u8/dash) → chuyển sang yt-dlp/ffmpeg ở Chapter 10.
  - **Vì sao cần cho OSINT sau này:**  
    - Nhiều video không tải được bằng “Save as” vì là stream (m3u8/dash).  
    - Stream Detector giúp em nhận diện loại stream và URL → chuyển sang yt-dlp/ffmpeg để tải, phục vụ thu thập chứng cứ video đúng cách.

- **Exif Viewer**
  - Mục đích: xem metadata nhanh.
  - **Vì sao cần cho OSINT sau này:**  
    - Giúp kiểm tra nhanh ảnh có metadata hay không (camera, thời gian, GPS...).  
    - Lưu ý thực tế: ảnh từ mạng xã hội thường bị xóa EXIF, nên Exif Viewer dùng để “check nhanh”, còn phân tích sâu sẽ dùng exiftool ở Chapter 10.

- **User-Agent Switcher and Manager**
  - Mục đích: giả lập mobile/desktop để thấy UI khác, nội dung khác, hoặc bypass chặn đơn giản.
  - **Vì sao cần cho OSINT sau này:**  
    - Một số nền tảng hiển thị nội dung khác nhau giữa mobile/desktop (UI, link, cách tải media).  
    - Có thể hỗ trợ truy cập “phiên bản nhẹ” để dễ capture, hoặc kiểm tra nội dung bị ẩn theo thiết bị.  
    - Dùng để kiểm thử giả thuyết: “nội dung có bị chặn theo client không?” trước khi chuyển sang phương án khác.

  <img width="844" height="295" alt="image" src="https://github.com/user-attachments/assets/07d07eb8-439e-40ac-add6-3a5e9396613b" />
  <img width="933" height="312" alt="image" src="https://github.com/user-attachments/assets/1b6ac902-79af-49ed-9840-d98e7dd7504f" />



### 4.4 Nhóm “tăng tốc quy trình”
> **Vì sao nhóm “tăng tốc” vẫn quan trọng trong OSINT:**  
> - OSINT thường phải thu thập số lượng lớn URL và đối chiếu nguồn → copy link hàng loạt giúp em dựng timeline và lưu nguồn nhanh hơn.  
> - Reverse image search 1-click rút ngắn thời gian xác minh ảnh (tìm ảnh gốc, ảnh bị tái sử dụng, ảnh từ nguồn khác).  
> - OneTab giúp quản lý tabs theo từng case, giảm lộn xộn và giảm rủi ro “đang case A lại mở nhầm tab case B”.
- **Copy Selected Links**: copy toàn bộ hyperlink trên trang.
- **Image Search Options** / **Search By Image**: reverse image search 1-click.
- **OneTab**: gom tabs để đỡ loạn và giảm RAM.
  
---

## 5) Export/Import cấu hình Firefox (để tái lập VM)
> **Vì sao cần cho OSINT sau này:**  
> - Khi build lại VM hoặc chuyển máy, em chỉ cần import profile là có ngay môi trường điều tra giống hệt (extensions, containers, settings).  
> - Giúp quy trình điều tra nhất quán: “lần nào cũng cấu hình như vậy” → dễ giải trình và dễ kiểm tra chéo.  
> - Đây cũng là cách tạo “baseline OSINT VM” để dùng lâu dài mà không phải cài lại từ đầu.
- Mục tiêu: cài 1 lần, clone/backup dùng lại.
<img width="1249" height="696" alt="image" src="https://github.com/user-attachments/assets/a6d8125c-838d-45b8-8efe-eb99b107ef78" />

---

## 6) Kết luận
Sau Chapter 9, em đã:
- Có Firefox cấu hình chuẩn OSINT,
- Có add-ons phục vụ thu thập chứng cứ,
- Biết export/import profile để tái lập môi trường.
