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

## 5) Thực hành
<img width="871" height="619" alt="image" src="https://github.com/user-attachments/assets/5da80098-3b96-4974-bd1e-a2ec10f83cbc" />
<img width="903" height="630" alt="image" src="https://github.com/user-attachments/assets/239733d5-47dc-4e85-8860-1ad9c59625d7" />
<img width="876" height="633" alt="image" src="https://github.com/user-attachments/assets/0ef4ff1d-abe8-45cc-a8ba-6a0344a2bd87" />
<img width="908" height="739" alt="image" src="https://github.com/user-attachments/assets/eee9b73d-632f-40b8-91ca-68885000b7c1" />
<img width="818" height="622" alt="image" src="https://github.com/user-attachments/assets/f8243fa9-b3ff-4575-9ada-a28e6150e405" />
<img width="914" height="668" alt="image" src="https://github.com/user-attachments/assets/d58f57f3-de22-4ca9-9861-756c67952fe9" />
<img width="976" height="685" alt="image" src="https://github.com/user-attachments/assets/c436d29c-4055-4268-be42-20e3aec3a529" />
<img width="834" height="632" alt="image" src="https://github.com/user-attachments/assets/ed085bad-7888-4cb2-a46b-8856e3f8b544" />
<img width="1912" height="947" alt="image" src="https://github.com/user-attachments/assets/b4b8cca5-77c6-4238-af82-45cb34bad38a" />
<img width="379" height="106" alt="image" src="https://github.com/user-attachments/assets/78448293-7185-4b53-9bfe-aaa8fb2030fb" />

