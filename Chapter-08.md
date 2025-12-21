# OSINT TECHNIQUES (CHAPTER 8)

## 1) Mục tiêu Chapter 8
- Dựng **Debian Stock** trên VM
- Cài desktop theo hướng dẫn (PDF chọn **GNOME**)
- Tạo baseline “Debian Stock” để clone/snapshot cho các chapter sau (Preserve Stock Debian)

## 2) Môi trường
- Hypervisor: VMware Workstation (Windows host)
- OS cài đặt: Debian (netinst ISO)
- Desktop: GNOME (theo PDF)
- Network: NAT

## 3) Các bước đã làm theo PDF
### 3.1 Debian installer
- Boot ISO → **Graphical install**
- Domain name: để trống
- Partition: Guided → use entire disk → ext4 + swap → Finish + write changes
- Scan extra installation media: **No**
- Anonymous statistics / popularity-contest: **No**
- Tasksel (Software selection):
  - [x] Debian desktop environment
  - [x] GNOME
  - [x] standard system utilities
  - [ ] web server, [ ] SSH server, [ ] desktop khác

### 3.2 GRUB
- Install GRUB boot loader: **Yes**
- Install to: **/dev/sda**

## 4) Sau khi cài xong
- Update hệ thống:
  - `sudo apt update && sudo apt full-upgrade -y`

## 5) Thực hành
<img width="976" height="685" alt="image" src="https://github.com/user-attachments/assets/c436d29c-4055-4268-be42-20e3aec3a529" />
<img width="1912" height="947" alt="image" src="https://github.com/user-attachments/assets/b4b8cca5-77c6-4238-af82-45cb34bad38a" />
<img width="379" height="106" alt="image" src="https://github.com/user-attachments/assets/78448293-7185-4b53-9bfe-aaa8fb2030fb" />

