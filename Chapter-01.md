# OSINT TECHNIQUES (CHAPTER 1)
**Ngày:** 21/11/2025
**Nội dung:** Xây dựng môi trường điều tra an toàn (The Sandbox)

---

## 1. TƯ DUY CỐT LÕI: TẠI SAO PHẢI LÀ "MÁY ẢO TỰ BUILD"?

Sau khi đọc chương 1, em nhận thấy tác giả khuyên không nên dùng các bản Linux làm sẵn (như Buscador cũ) nữa, vì chúng đã ngừng cập nhật từ lâu và tiềm ẩn rủi ro bảo mật.

Việc tự tay cài đặt từng công cụ trên máy ảo giúp em:
1.  **Kiểm soát hoàn toàn:** Biết chính xác mình đang chạy cái gì, không sợ bị cài phần mềm gián điệp ẩn.
2.  **Hiểu sâu:** Hiểu cách công cụ hoạt động thay vì chỉ bấm chuột máy móc.

Tự xây dựng để kiểm soát hệ thống. Điều này đảm bảo:
*   **Clean Environment:** Cách ly hoàn toàn mã độc và che giấu danh tính thật.
*   **Control:** Chủ động cập nhật hoặc tiêu hủy môi trường ngay lập tức khi gặp sự cố.

## 2. THỰC HÀNH: THIẾT LẬP PHÒNG LAB

Em đã hoàn tất việc setup Kali Linux 2025.3 trên VMware Workstation 17 Pro. Quan trọng nhất, em đã thiết lập quy trình **Snapshot** để đảm bảo an toàn dữ liệu (Cơ chế: *Investigate -> Destroy -> Revert*).

<img width="1919" height="1014" alt="image" src="https://github.com/user-attachments/assets/33205997-19db-4732-a883-4a646dda0550" />
*(Hình ảnh: Giao diện VMware với máy ảo Kali Linux đã được cấu hình và trạng thái Snapshot)*

## 3. CHECKLIST

- [x] Máy ảo khởi động thành công, kết nối internet ổn định.
- [x] Hiểu cơ chế Snapshot/Restore để xóa dấu vết.
- [x] Tách biệt hoàn toàn dữ liệu máy thật và máy ảo.
