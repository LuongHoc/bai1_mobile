# Họ và tên: Lương Văn Học  - MSSV:K225480106025
# Lớp: K58KTP
# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421
# Bài tập 1:

---

# A. Đăng ký tên miền xịn cho cá nhân:
## 1. Đăng kí domain 
Link đăng kí domain: https://tenten.vn/

Tên domain đăng kí: luongvanhoc.io.vn

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e496273-550c-4899-9d96-11207c5f6d3f" />

## 2.Đăng ký tài khoản cloudflare

Bước 1: Truy cập trang đăng ký

- Vào đường dẫn: https://dash.cloudflare.com/sign-up

- Sẽ thấy form đăng ký

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ccde5810-0761-4ad2-a56c-452076064ed0" />

Bước 2: Sẽ thấy các lựa chọn:

- Sign up bằng email

- Continue with GitHub 

- Tạo tài khoản mới

**Ở đây em dùng đăng nhập bằng GitHub**

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8abffabb-431f-42c2-b21f-40874cd51698" />

## 3.Thêm domain đã đăng ký vào trong cloudflare 

### Bước1: Vào Domains → Overview
1. Ở menu bên trái, chọn **Domains**.
2. Chọn **Overview**.
4. Tìm và bấm **Add a site**.
<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f43cc6c-4bca-4ab2-929e-47236185dc91" />

## Bước 2: Nhập tên miền (domain)
1. Ở màn hình **Add a site**, tại ô “Enter an existing domain…”, nhập **domain gốc**
   - `luongvanhoc.io.vn`
2. Chọn cách thêm DNS records:
   - Chọn **Import DNS records automatically**.
4. (Tuỳ chọn) Phần “Block AI training bots” có thể để mặc định hoặc chọn “Do not block”.
5. Bấm **Continue**.

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/68c33055-5e24-4298-9df5-3b39255848c4" />

### Bước 3: Xác nhận DNS records (Confirm scanned records)
1. Cloudflare sẽ chuyển sang trang xác nhận các DNS records đã quét.

3. Bấm nút **Continue to activation**.

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/76c50583-0eb7-4f41-813e-b478c22e5713" />

### Bước 4: Nhận 2 dòng Nameserver (Namespace) do Cloudflare cấp
1. Cloudflare sẽ hiển thị trang: **Update your nameservers to activate Cloudflare**.
2. Ở mục “Replace your current nameservers with Cloudflare nameservers”, sẽ thấy **2 dòng nameserver** do Cloudflare cấp:
   - `clayton.ns.cloudflare.com`
   - `evelyn.ns.cloudflare.com`
<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/d5136e98-9b5d-4651-9a6a-4127e3e7e70b" />

## 4. Nhập 2 dòng namespace của cloudflare vào trong trang quản lý DNS record của tên miền đăng ký

### Bước 1: Đăng nhập trang quản lý domain ở nhà đăng ký
1. Mở trang quản lý dịch vụ của nhà đăng ký domain (TenTen).
2. Đăng nhập tài khoản.
3. Vào **Quản lý tên miền / Domain**.
4. Chọn đúng domain cần đổi: `luongvanhoc.io.vn`

### Bước 2: Mở chức năng “Cài đặt NS / Nameserver”
1. Trong danh sách domain, tìm nút bánh răng “Quản trị”.
2. Chọn mục: **Cài đặt NS**

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/76b2f3b9-afd6-4901-b8a3-a8a4205db589" />

### Bước 3: Thay NS cũ bằng 2 NS Cloudflare
1. Trong cửa sổ/biểu mẫu “Cập nhật Nameserver”, sẽ thấy các ô **NS1, NS2, NS3...**
2. Xóa nameserver cũ.
3. Nhập đúng 2 dòng Cloudflare:
   - **NS1:** `clayton.ns.cloudflare.com`
   - **NS2:** `evelyn.ns.cloudflare.com`
4. Nhấn **Cập nhật**.

<img width="1980" height="1035" alt="image" src="https://github.com/user-attachments/assets/ff131512-3bc5-49ba-8c43-a497277dd969" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c3ac013e-0bb3-4d3e-a13f-90b074fc0844" />

### Bước 4: Xác nhận trên Cloudflare
1. Quay lại Cloudflare.
2. Chờ DNS cập nhật
4. Khi xong, vào Cloudflare → **Domains → Overview**:
   - Domain hiển thị trạng thái **Active** là hoàn tất.

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/8c31a0d7-93a3-4282-850d-6769b06e9f58" />

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

### Bước 5. Đăng nhập Ubuntu Server và lấy IP

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

































