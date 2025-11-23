# OSINT TECHNIQUES (CHAPTER 14)
**Trạng thái:** HOÀN THÀNH (REVIEW ONLY)

## 1. OSINT VM AUTOMATED BUILD
- **Nội dung:** Tác giả cung cấp script tự động (`install.sh`) để biến một máy Debian trắng thành máy OSINT hoàn chỉnh (cài tool, config Firefox, v.v.).
- **Lệnh gốc:** `wget https://uvm:317@inteltechniques.com/osintvm/install.sh`

### Quyết định kỹ thuật (Decision):
- [x] **SKIP EXECUTION:** Vì em đang sử dụng **Kali Linux** (đã tích hợp sẵn nhiều tool và cấu hình riêng), việc chạy script dành cho Debian có thể gây vỡ hệ thống (broken dependencies/conflicts).
- [x] **SOURCE CODE REVIEW:** Đã tải script về chỉ để tham khảo danh sách tool/cấu hình.
- [ ] **Manual Check:** Nếu phát hiện tool nào hay trong script mà Kali chưa có -> Cài tay.

## 2. BẰNG CHỨNG / KẾT QUẢ
- Đã lưu file `install.sh` vào thư mục `~/Documents/OSINT-Scripts/` để tham khảo sau này.
- Hệ thống Kali vẫn hoạt động ổn định, không bị ghi đè cấu hình.
