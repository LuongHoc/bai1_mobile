# B. Cài đặt Ubuntu + Docker

## Cài đặt hệ điều hành Ubuntu 24.04.4 LTS + SSH từ Windows vào Ubuntu

### Bước 1. Chuẩn bị
- Tải file ISO: **Ubuntu 24.04.4 LTS (Desktop)** từ trang Ubuntu.

Link tải: https://releases.ubuntu.com/24.04.4/

- Cài công cụ ảo hóa: **VMware Workstation**

link tải:https://download.com.vn/vmware-workstation-8587

### Bước 2. Tạo máy ảo Ubuntu trên VMware

1. **Create a New Virtual Machine**

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/3339b99e-091a-4c48-a0d5-68944bc5eaa4" />

2. Chọn **Installer disc image file (iso)** → trỏ đến ISO Ubuntu Server

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/04e17bed-829d-4f1d-88c2-73fd2b537759" />

3. Guest OS: **Linux → Ubuntu 64-bit**
4. Cấu hình gợi ý:
   - CPU: 2 cores
   - RAM: 2–4 GB
   - Disk: 20–30 GB
   
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f527dc5d-cc7a-43bf-87a8-640c9d1bc7d6" />

5. **Network Adapter**: chọn **Bridged** (để IP cùng lớp mạng LAN)

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/f12f6a92-c20b-4894-ac11-36d6f52e08fc" />

6. **Finish** → **Power on** máy ảo

### Bước 3. Cài Ubuntu 24.04.4 Live Server (text installer)
Trong trình cài đặt:
1. GRUB: chọn **Try or Install Ubuntu Server**
2. Language: chọn **English**
3. Keyboard configuration:
   - Layout: **English (US)**
   - Variant: **English (US)**
   - Chọn **Done**
4. Network: để DHCP tự nhận IP → **Done**
5. Proxy configuration: để trống → **Done**
6. Storage:
   - Chọn **Use an entire disk**
   - (Có thể giữ mặc định LVM)
   - Chọn **Done** và xác nhận **Continue**
7. Profile configuration (tạo user để đăng nhập và SSH):
   - Your name: `Luong Van Hoc`
   - Your server’s name (hostname): `ubuntu-server`
   - Pick a username:`admin1`
   - Choose a password / Confirm: đặt mật khẩu
   - **Done**
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fc09b445-bb9e-4196-a55c-f9e208cfab54" />
8. Chờ cài xong → chọn **Reboot Now**

### Bước 4. Tháo ISO sau khi cài (tránh boot lại vào bộ cài)
Khi reboot nếu thấy yêu cầu “remove the installation medium” hoặc boot lại vào installer:
- VMware → **VM → Settings → CD/DVD**
  - bỏ tick **Connected**
  - bỏ tick **Connect at power on**
- Quay lại VM và nhấn **Enter** để tiếp tục reboot

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/7ff2b27b-1fa9-4628-8f94-9d6eb78f3052" />

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/f56325b8-e989-47a3-a99b-7619cedde823" />

### Bước 5. Đăng nhập Ubuntu Server cài SSH và lấy IP

1. Sau khi máy boot vào Ubuntu Server (tty), đăng nhập user đã tạo `admin1`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/51bee6f7-d3aa-409e-8daa-098ed79e3891" />

2. Cài và bật SSH server (nếu chưa có)
Kiểm tra service SSH:
```bash
sudo systemctl status ssh
```

Nếu báo `Unit ssh.service could not be found.` thì cài OpenSSH Server:
```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
sudo systemctl status ssh
```

Xác nhận thành công khi thấy:
- `Active: active (running)`
- `Server listening on 0.0.0.0 port 22`
Kiểm tra IP:
```bash
ip -4 addr
```

Kết quả thực tế:
- IPv4 của `ens33`: **192.168.1.16**

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/e7c262ed-ce25-4fd7-a4c4-9debe2e9eb0c" />


### Bước 6. SSH từ Windows CMD vào Ubuntu
Trên Windows mở **CMD** và chạy:
```bat
ssh admin1@192.168.1.16
```

Lần đầu kết nối sẽ hỏi xác nhận fingerprint:
- gõ `yes` → Enter  
Sau đó nhập password (lưu ý password **không hiển thị**) → Enter

Kết quả sau khi SSH thành công sẽ thấy:
- `Welcome to Ubuntu 24.04.4 LTS ...`
- prompt dạng: `admin1@ubuntu-server:~$`

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/032cccc5-155f-48e7-9981-a03b7bc18013" />

