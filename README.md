# Họ và tên: Lương Văn Học  - MSSV:K225480106025
# Lớp: K58KTP
# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421
# Bài tập 1:

---

# A. Đăng ký tên miền xịn cho cá nhân:
## 1. Đăng kí domain 
link đăng kí domain: https://tenten.vn/

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
2. Chọn **Installer disc image file (iso)** và trỏ đến file ISO Ubuntu.

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/3339b99e-091a-4c48-a0d5-68944bc5eaa4" />

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/80a3349a-e5dc-4ccf-9f25-43f312a7f95c" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8832cede-0bf3-4554-8aff-07797dacf92d" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/584790f8-6e7a-4d56-94f5-8e274998f3bb" />

3. Cấu hình tối thiểu gợi ý:
   - CPU: 2 cores
   - RAM: 4 GB
   - Disk: 30 GB (hoặc hơn)
4. Bật máy ảo và bắt đầu cài Ubuntu.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9e135fe7-4d43-49db-ac92-68d5411183d4" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8c434b83-efa6-44ab-a13b-225c06ec06c3" />

### Bước 3. Cài đặt Ubuntu 24.04.4 LTS (Desktop)
Trong trình cài đặt Ubuntu:
1. Chọn ngôn ngữ, bàn phím **English (US)**.
2. Tới bước tạo user:
   - **Your name**:`Luong Van Hoc`
   - **Your computer’s name**:`ubuntu-vm`
   - **Your username**: `admin1` (lưu ý: một số bản cài Desktop không cho dùng `admin` vì reserved)
   - Đặt mật khẩu
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a81c4c32-b8e1-4803-bf3d-ffca4cdc3419" />
3. Disk setup: chọn **Erase disk and install Ubuntu**  
   > Chỉ xóa **ổ đĩa ảo** trong VM, không ảnh hưởng Windows thật.
4. Cài đặt xong chọn **Restart now**.

**Quan trọng:** Sau khi restart, nếu VM boot lại vào màn cài đặt, hãy tháo ISO:
- VMware → **VM Settings → CD/DVD** → bỏ tick:
  - `Connected`
  - `Connect at power on`
- Reboot lại VM.








<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/38c0c45c-a7b7-4314-a03e-809311290aa3" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a4b13d05-8a4c-46bb-a426-07d7ed581c2e" />



<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/bbf9ba34-ce67-495c-95fc-38d7933d6492" />



<img width="1105" height="640" alt="image" src="https://github.com/user-attachments/assets/0fb9294b-9736-4af4-8613-b7bce425c759" />











---



---

## 4) Cấu hình mạng VMware để Ubuntu có IP LAN (192.168.x.x)
Trong VMware:
- **VM → Settings → Network Adapter**
  - Tick `Connected`
  - Tick `Connect at power on`
  - Chọn **Bridged: Connected directly to the physical network**
  - (Không bắt buộc tick `Replicate physical network connection state`)

Boot vào Ubuntu, kiểm tra IP sau (mục 6).

---

## 5) Cài và bật SSH Server trên Ubuntu
Mở Terminal trên Ubuntu (Ctrl+Alt+T) và chạy:

```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
sudo systemctl status ssh
```

Nếu `status` hiển thị `Active: active (running)` là SSH đã chạy.

> Thoát màn hình status: nhấn `q` (không gõ `:q`).

---

## 6) Lấy địa chỉ IP của Ubuntu
Chạy:

```bash
ip -4 addr
```

Ghi lại dòng có dạng `inet 192.168...` tại card mạng (thường là `ens33`), ví dụ:

- `inet 192.168.1.10/24 ...`  → IP SSH là **192.168.1.10**

---

## 7) SSH từ Windows CMD vào Ubuntu
Trên Windows mở **CMD** và gõ:

```bat
ssh <username>@<ip_ubuntu>
```

Ví dụ với user `admin1` và IP `192.168.1.10`:

```bat
ssh admin1@192.168.1.10
```

Lần đầu SSH sẽ hỏi xác nhận host key:

- Gõ `yes` → Enter
- Nhập mật khẩu (mật khẩu **không hiện**) → Enter

Khi thành công sẽ thấy:

- `Welcome to Ubuntu 24.04.4 LTS ...`
- prompt dạng `admin1@ubuntu-vm:~$`

=> Hoàn thành yêu cầu SSH từ Windows vào Ubuntu.

---

## 8) (Tuỳ yêu cầu bài) Nếu bắt buộc user phải là `admin`
Nếu đề yêu cầu đúng cú pháp `ssh admin@<ip>`, tạo thêm user `admin` trên Ubuntu:

```bash
sudo adduser admin
sudo usermod -aG sudo admin
```

Sau đó trên Windows SSH lại:

```bat
ssh admin@192.168.1.10
```

---

## Kết quả đạt được (B1)
- Ubuntu 24.04.4 LTS đã cài trên VM.
- Ubuntu có IP LAN (ví dụ `192.168.1.10`).
- SSH server đã bật (port 22).
- Windows CMD SSH vào Ubuntu thành công bằng `ssh <user>@<ip>`.


<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/a5fb2def-46a6-4611-9ae3-decfa5e6dcb6" />

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/5ed1ecbb-3573-4e2b-9637-a3ebd447ee8e" />

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/62c1aa86-406d-45c9-b24d-a15516706192" />


<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/3e4320b7-1097-40b3-8bfd-eb07510ee993" />

