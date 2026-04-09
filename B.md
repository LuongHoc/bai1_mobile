# B. Cài đặt Ubuntu + Docker

# B-1. Cài đặt hệ điều hành Ubuntu 24.04.4 LTS + SSH từ Windows vào Ubuntu

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


# B-2. Tìm hiểu các lệnh cơ bản của ubuntu

### 1) Đăng nhập Ubuntu (khuyến nghị qua SSH)
Từ Windows CMD:

```bat
ssh admin1@192.168.1.16
```

Sau khi đăng nhập thành công sẽ thấy prompt dạng:
```text
admin1@ubuntu-server:~$
```

## 2) Thực hành các lệnh

### Bước 0 — Kiểm tra thư mục hiện tại + liệt kê file
```bash
pwd
ls
ls -l
```
<img width="1104" height="641" alt="image" src="https://github.com/user-attachments/assets/439ec8f3-5774-4b63-81fb-da1b8ab202ef" />

### Bước 1 — Tạo thư mục làm bài
Tạo thư mục `B2` và thư mục con `data`:
```bash
mkdir -p B2/data
```

Kiểm tra:
```bash
ls -l
```

<img width="606" height="264" alt="image" src="https://github.com/user-attachments/assets/a74584cc-d770-420c-a817-b44186ae9656" />


### Bước 2 — Chuyển thư mục làm việc (cd)
```bash
cd B2
pwd
ls
```
<img width="1105" height="640" alt="image" src="https://github.com/user-attachments/assets/0561c101-ec3d-47fc-a9e6-8988e04fb79c" />

### Bước 3 — Tạo và sửa file bằng nano
Tạo file `note.txt`:
```bash
nano note.txt
```

Nhập nội dung ví dụ:
- `Day la bai B2`
- `User: admin1`
- `Ngay: 2026-04-09`

Lưu và thoát:
- Nhấn `CTRL + O` → nhấn `Enter` để xác nhận lưu
- Nhấn `CTRL + X` để thoát

<img width="1102" height="638" alt="image" src="https://github.com/user-attachments/assets/605f7126-7dfa-425e-91f9-d6260b038b1c" />

Kiểm tra file:
```bash
ls -l
```
<img width="1105" height="641" alt="image" src="https://github.com/user-attachments/assets/6bd2ff42-7e13-4793-8ed6-da3090d82808" />

### Bước 4 — Copy file (cp)
Copy `note.txt` sang thư mục `data` với tên mới `note_copy.txt`:
```bash
cp note.txt data/note_copy.txt
```

Kiểm tra:
```bash
ls -l
ls -l data
```
<img width="1104" height="634" alt="image" src="https://github.com/user-attachments/assets/f654c3da-99f5-4c13-8473-f67aedaa61ee" />

### Bước 5 — Thay đổi quyền file (chmod)
Xem quyền hiện tại:
```bash
ls -l note.txt
```

Đổi quyền `note.txt` thành `644`:
```bash
sudo chmod 644 note.txt
ls -l note.txt
```

Đổi quyền file copy thành `777` (chỉ dùng cho bài lab):
```bash
sudo chmod 777 data/note_copy.txt
ls -l data/note_copy.txt
```

<img width="1103" height="641" alt="image" src="https://github.com/user-attachments/assets/2db29e4b-43ad-4be1-a352-0908943107f6" />

### Bước 6 — Edit file bằng `sudo nano`
```bash
sudo nano data/note_copy.txt
```

Thêm 1 dòng ví dụ:
- `Da sua file bang sudo nano`

Lưu và thoát:
- `CTRL + O` → `Enter`
- `CTRL + X`

<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/03b230dd-3e01-4791-9c63-8f7a19ddd171" />

### Bước 7 — Xem IP của máy Ubuntu
```bash
ip -4 addr
```
<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/181a57ac-1fc0-4e46-a6da-6754e4728409" />


# B-3. Cài đặt Docker cho Ubuntu 24.04.4 LTS

### 1) Cập nhật hệ thống và cài gói cần thiết
```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
```

### 2) Thêm GPG key của Docker
```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

> Nếu xuất hiện:  
> `File '/etc/apt/keyrings/docker.gpg' exists. Overwrite? (y/N)`  
> thì gõ `y` rồi Enter để ghi đè.


### 3) Thêm Docker repository 
```bash
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo $VERSION_CODENAME) stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Cập nhật lại package list:
```bash
sudo apt update
```

<img width="796" height="639" alt="image" src="https://github.com/user-attachments/assets/632b4242-523a-43bd-b052-6e880461c1ec" />

### 4) Cài Docker Engine + Docker Compose plugin
```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Bật Docker chạy ngay và tự khởi động cùng hệ thống:
```bash
sudo systemctl enable --now docker
```


# B-4. Kiểm tra Docker đã cài thành công
```bash
docker --version
docker compose version
sudo systemctl status docker
```

Kết quả:
- Có version của Docker
- Có version của `docker compose`
- `systemctl status docker` báo `Active: active (running)`  
  (nhấn `q` để thoát màn hình status)
  
<img width="1105" height="638" alt="image" src="https://github.com/user-attachments/assets/ca1ea930-643a-4198-b765-e61cd9d496c6" />


# B-5. Cấu hình chạy Docker không cần `sudo`
Thêm user hiện tại vào nhóm `docker`:
```bash
sudo usermod -aG docker $USER
```

Áp dụng group mới (không cần reboot):
```bash
newgrp docker
```

Kiểm tra nhóm hiện tại:
```bash
groups
```
Kỳ vọng có chữ `docker`.

Test chạy Docker không cần sudo:
```bash
docker run --rm hello-world
```

Nếu thấy:
```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```
=> Docker hoạt động đúng.

<img width="1102" height="638" alt="image" src="https://github.com/user-attachments/assets/93098318-57cb-4f36-97fa-fa8a3039e37d" />

# B-6. Tìm hiểu tập lệnh của docker và docker compose

## 1. Docker

### 1.1 Kiểm tra phiên bản và thông tin Docker
```bash
docker --version
docker version
docker info
```

- `docker --version`: xem phiên bản Docker client
- `docker version`: xem chi tiết phiên bản client/server
- `docker info`: thông tin tổng quan Docker Engine (storage driver, số container, network…)

### 1.2 Quản lý Image
```bash
docker images
docker pull nginx:latest
docker rmi nginx:latest
docker image prune -a
```

- `docker images`: liệt kê các image đang có
- `docker pull <image>:<tag>`: tải image từ registry (thường là Docker Hub)
- `docker rmi <image>`: xoá image
- `docker image prune -a`: dọn image không dùng (**cẩn thận** vì có thể xoá nhiều image)


### 1.3 Quản lý Container
```bash
docker ps
docker ps -a
docker run --name web -d -p 80:80 nginx:latest
docker logs web
docker exec -it web bash
docker stop web
docker start web
docker restart web
docker rm web
```

Giải thích nhanh:
- `docker ps`: xem container đang chạy
- `docker ps -a`: xem tất cả container (kể cả đã dừng)
- `docker run`: tạo + chạy container  
  Ví dụ: `-d` chạy nền, `--name web` đặt tên, `-p 80:80` map cổng host:container
- `docker logs <name>`: xem log
- `docker exec -it <name> bash`: vào container để thao tác
- `docker stop/start/restart`: dừng / chạy / restart container
- `docker rm <name>`: xoá container (phải stop trước, hoặc dùng `-f`)

### 1.4 Quản lý Volume (dữ liệu bền vững)
```bash
docker volume ls
docker volume create mydata
docker volume inspect mydata
docker volume rm mydata
```

Ghi chú:
- Volume giúp dữ liệu **không mất** khi xoá container.
- Thường dùng cho database, ứng dụng cần lưu dữ liệu.

### 1.5 Quản lý Network (mạng cho container)
```bash
docker network ls
docker network create mynet
docker network inspect mynet
docker network rm mynet
```

Ghi chú:
- Network giúp các container giao tiếp với nhau qua tên container/service.
- Khi dùng Docker Compose, network thường được tạo tự động.


## 2) Docker Compose

### 2.1 Kiểm tra phiên bản
```bash
docker compose version
```

### 2.2 Các lệnh Compose cơ bản
```bash
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
```

Giải thích:
- `up -d`: tạo và chạy tất cả service theo file `docker-compose.yml` / `compose.yaml`
- `ps`: xem trạng thái các service/container
- `logs -f`: xem log realtime
- `down`: dừng và xoá container + network do compose tạo  
  (muốn xoá cả volume dùng `docker compose down -v`)

### 2.3 Build image (khi có Dockerfile)
```bash
docker compose build
docker compose up -d --build
```

### 2.4 Stop / Start / Restart theo stack compose
```bash
docker compose stop
docker compose start
docker compose restart
```


### 2.5 Xem cấu hình sau khi compose xử lý biến môi trường
```bash
docker compose config
```

# B-7. Mở firewall UFW cho cổng 80, 1880, 9630

## 1) Kiểm tra trạng thái UFW
```bash
sudo ufw status
```

- Nếu kết quả là `Status: inactive`: UFW đang tắt (chưa chặn gì).
- Nếu kết quả là `Status: active`: UFW đang bật (đang áp dụng rule).

<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/ca612286-662c-4091-9520-c346417cac02" />


## 2) Cho phép SSH để tránh mất kết nối
Vì thao tác qua SSH nên cần cho phép SSH (port 22) trước khi bật UFW:
```bash
sudo ufw allow OpenSSH
```

<img width="1106" height="639" alt="image" src="https://github.com/user-attachments/assets/2e1b25ed-b724-4eb5-81d7-6026c2f24509" />


## 3) Mở các cổng yêu cầu: 80, 1880, 9630
```bash
sudo ufw allow 80/tcp
sudo ufw allow 1880/tcp
sudo ufw allow 9630/tcp
```
<img width="1103" height="638" alt="image" src="https://github.com/user-attachments/assets/b252ca74-d7c6-4f0d-b2b1-166a5793a076" />

## 4) Bật UFW và cho phép tự khởi động cùng hệ thống
```bash
sudo ufw enable
```

Khi hệ thống hỏi:
```text
Proceed with operation (y|n)?
```
gõ `y` rồi Enter.

<img width="1104" height="642" alt="image" src="https://github.com/user-attachments/assets/bca12d8b-0840-40ce-af66-fd80a42a1e97" />

## 5) Kiểm tra lại rule đã được áp dụng
```bash
sudo ufw status numbered
```

<img width="1105" height="646" alt="image" src="https://github.com/user-attachments/assets/155c77fd-8124-4969-9cd6-5c56c4518042" />




