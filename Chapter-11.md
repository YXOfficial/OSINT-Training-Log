# OSINT TECHNIQUES (CHAPTER 11)
**Automating OSINT VM: Custom Scripts & Shortcuts**

> **Mục tiêu Chapter 11:**
> Chuyển đổi từ việc gõ lệnh thủ công (Chapter 10) sang sử dụng **Giao diện đồ họa (GUI)** tự tạo.
> - Tạo các Bash Script để tự động hóa quy trình.
> - Sử dụng `Zenity` để tạo Menu chọn lựa.
> - Tích hợp Shortcut vào hệ thống Debian để thao tác "1 click".

---

## 1. THIẾT LẬP MÔI TRƯỜNG & TẢI SCRIPTS

Thay vì gõ lệnh `yt-dlp` dài dòng, em sẽ tải các script do Michael Bazzell viết sẵn. Các script này đóng vai trò là "lớp vỏ" (wrapper) gọi các lệnh em đã cài ở Chapter 10.

### Bước 1.1: Tạo thư mục và tải Script `.sh`
Chạy các lệnh sau trong Terminal để kéo file từ server của IntelTechniques về (dựa theo trang PDF):

```bash
mkdir -p ~/Documents/scripts
cd ~/Documents/scripts

# Tải các script xử lý chính
curl -O https://uvm:317@inteltechniques.com/osintvm/api.sh
curl -O https://uvm:317@inteltechniques.com/osintvm/domain.sh
curl -O https://uvm:317@inteltechniques.com/osintvm/framework.sh
curl -O https://uvm:317@inteltechniques.com/osintvm/image.sh
curl -O https://uvm:317@inteltechniques.com/osintvm/metadata.sh
curl -O https://uvm:317@inteltechniques.com/osintvm/update.sh
curl -O https://uvm:317@inteltechniques.com/osintvm/user.sh
curl -O https://uvm:317@inteltechniques.com/osintvm/video.sh

# Cấp quyền thực thi (executable) cho script
chmod +x *.sh
```

<img width="504" height="192" alt="image" src="https://github.com/user-attachments/assets/98431cbe-e210-44b2-aa6c-d38faaaade1a" />

---

## 2. CẤU HÌNH SHORTCUT & ICON (DESKTOP ENTRY)

Để đưa script lên thanh Menu/Dock, em cần file `.desktop` và `.png` (icon).

### Bước 2.1: Tải file cấu hình và icon
(Dựa theo trang 140 PDF)

```bash
# Tải file shortcut (.desktop)
curl -O https://uvm:317@inteltechniques.com/osintvm/api.desktop
curl -O https://uvm:317@inteltechniques.com/osintvm/domain.desktop
curl -O https://uvm:317@inteltechniques.com/osintvm/framework.desktop
curl -O https://uvm:317@inteltechniques.com/osintvm/image.desktop
curl -O https://uvm:317@inteltechniques.com/osintvm/metadata.desktop
curl -O https://uvm:317@inteltechniques.com/osintvm/search.desktop
curl -O https://uvm:317@inteltechniques.com/osintvm/update.desktop
curl -O https://uvm:317@inteltechniques.com/osintvm/user.desktop
curl -O https://uvm:317@inteltechniques.com/osintvm/video.desktop

# Tải Icon (.png)
curl -O https://uvm:317@inteltechniques.com/osintvm/api.png
curl -O https://uvm:317@inteltechniques.com/osintvm/domain.png
curl -O https://uvm:317@inteltechniques.com/osintvm/framework.png
curl -O https://uvm:317@inteltechniques.com/osintvm/image.png
curl -O https://uvm:317@inteltechniques.com/osintvm/metadata.png
curl -O https://uvm:317@inteltechniques.com/osintvm/search.png
curl -O https://uvm:317@inteltechniques.com/osintvm/update.png
curl -O https://uvm:317@inteltechniques.com/osintvm/user.png
curl -O https://uvm:317@inteltechniques.com/osintvm/video.png
```

### Bước 2.2: Cài đặt vào hệ thống
Di chuyển các file `.desktop` vào thư mục ứng dụng của hệ điều hành:

```bash
sudo mv *.desktop /usr/share/applications/
```

<img width="721" height="54" alt="image" src="https://github.com/user-attachments/assets/9998a4a1-e4b2-4549-ba2e-f3a3b01c5b6f" />


---

## 3. FIX LỖI ĐƯỜNG DẪN

Sách giả định user tên là `osint` (`/home/osint/`). Vì user của em khác, script sẽ không chạy. Em phải sửa lại đường dẫn trỏ về đúng user hiện tại (`$USER`).

### Bước 3.1: Sửa đường dẫn trong Script (.sh)

```bash
cd ~/Documents/scripts
sed -i "s|/home/osint/|$HOME/|g" *.sh
```

### Bước 3.2: Sửa đường dẫn trong Shortcut (.desktop)

```bash
cd /usr/share/applications/
sudo sed -i "s|/home/osint/|$HOME/|g" api.desktop domain.desktop framework.desktop image.desktop metadata.desktop search.desktop update.desktop user.desktop video.desktop
```

<img width="717" height="143" alt="image" src="https://github.com/user-attachments/assets/cf5fb3e6-cdde-4daf-a8a0-ce959a071c89" />

---

## 4. TÙY BIẾN SCRIPT "UPDATE TOOL"

Trong Chapter 10, em đã dùng **pipx** để cài tool Python. Tuy nhiên, script `update.sh` gốc của sách lại dùng `git pull` thủ công (Trang 141). Nếu chạy script gốc, nó sẽ lỗi hoặc tạo ra các bản sao rác. Em cần viết lại script này.

### Bước 4.1: Chỉnh sửa `update.sh`
Mở file `~/Documents/scripts/update.sh` và sửa thành nội dung sau (tối ưu cho `pipx`):

```bash
#!/usr/bin/env bash

# 1. Update hệ thống Debian
echo "--- UPDATING SYSTEM (APT) ---"
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y

# 2. Update các tool Python cài qua PIPX (Sherlock, Holehe, etc.)
echo "--- UPDATING PYTHON TOOLS (PIPX) ---"
pipx upgrade-all

# 3. Update Flatpak (nếu có dùng)
echo "--- UPDATING FLATPAK ---"
flatpak update -y

echo "Update process complete. Press Enter to exit."
read data
```

---

## 5. KẾT QUẢ & VERIFICATION

### Bước 5.1: Đưa icon ra Dock (Tùy chọn)
Chạy lệnh gsettings để ghim các tool này vào thanh Favorite (Trang 141):

```bash
gsettings set org.gnome.shell favorite-apps "['firefox-esr.desktop', 'org.gnome.Terminal.desktop', 'org.gnome.Nautilus.desktop', 'update.desktop', 'search.desktop', 'video.desktop', 'user.desktop', 'image.desktop', 'domain.desktop', 'metadata.desktop', 'framework.desktop', 'api.desktop']"
```
<img width="1349" height="429" alt="image" src="https://github.com/user-attachments/assets/7b24b376-912a-4943-b4f2-62f52ba12216" />


### Bước 5.2: Test chức năng (Video Tool)

<img width="873" height="832" alt="image" src="https://github.com/user-attachments/assets/dca53f79-8d8b-441e-b191-a05a77c517bd" />


---

## TỔNG KẾT CHAPTER 11

Em đã hoàn thiện về mặt chức năng:
1.  **Backend (Chapter 10):** Các công cụ mạnh mẽ (`yt-dlp`, `sherlock`, `exiftool`) đã được cài đặt qua `apt` và `pipx`.
2.  **Frontend (Chapter 11):** Các Script và Menu giúp thao tác nhanh, không cần nhớ cú pháp lệnh phức tạp.
3.  **Maintenance:** Script `update.sh` đã được tối ưu để giữ cho toàn bộ kho vũ khí luôn ở phiên bản mới nhất chỉ với 1 cú click.
