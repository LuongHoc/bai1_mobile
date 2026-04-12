# G. Triển khai ứng dụng đến End-user (Cloudflare Tunnel + Docker Compose)

## G.1) Tạo Cloudflare Tunnel (triển khai kiểu Docker)

1. Truy cập Cloudflare Zero Trust (Cloudflare One):  
   `https://one.dash.cloudflare.com`
2. Chọn đúng **Account**.

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/f53fe945-3230-428a-b17e-bbf1e236b591" />

3. Vào **Networks → Overview**.
4. Chọn **Manage Tunnels** →  **Create new cloudflared Tunnel**

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/1a0ddb97-c3d7-446a-b621-6c7c6ece7f11" />

5. Đặt tên tunnel (`myapp-tunnel`) → **Save tunnel**.

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/82a99c63-fee6-4696-8151-4f29d6cb5735" />

6. Ở bước **Install and run connectors**, chọn **Docker** và **copy Tunnel Token**

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/f47efcbe-0596-45f6-9255-98da6b0e9978" />


## G.2) Convert lệnh `docker run` sang `docker compose`

Cloudflare cung cấp lệnh mẫu dạng:

```bash
docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token eyJhIjoiZWNhNDdhZWZiNmMyZWM3MWVkMWY5ZmI3NDZjZmQwNzMiLCJ0IjoiOTAyMDUxMzQtMTVlNi00ZWYwLTkxMzUtNGVjODhiZDQ4YWQwIiwicyI6Ik1XRmhOemxpWTJFdFpHWTFNeTAwWXpJekxXRTVOamN0WW1SaE5UZG1PVGRoWWpZMCJ9
```

Thay vì chạy trực tiếp, ta đưa cấu hình này vào `docker-compose.yml` để quản lý cùng các container khác (nginx, nodered, myapi,…).


## G.3) Khai báo cloudflared vào `docker-compose.yml`

### 3.1 Tạo file `.env` để chứa token

Trong thư mục dự án, tạo file `.env`:

```bash
cd ~/myapp
nano .env
```
<img width="1105" height="643" alt="image" src="https://github.com/user-attachments/assets/e76cb4ef-bcb8-4b03-9142-507f1a3e8d2d" />

<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/ef9a5004-1d2f-459a-8460-fef5cf9f6f3e" />


### 3.2 Thêm service `cloudflared` vào `docker-compose.yml`

Mở file:

```bash
nano docker-compose.yml
```

Thêm service `cloudflared` :

```yaml
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    command: tunnel --no-autoupdate run --token ${CLOUDFLARE_TUNNEL_TOKEN}
    restart: unless-stopped
```

<img width="1097" height="538" alt="image" src="https://github.com/user-attachments/assets/3b3c5014-b095-4f76-a773-85881154c67c" />

Lưu file: **Ctrl+O → Enter → Ctrl+X**



## G.4) Chạy lại Docker Compose

```bash
cd ~/myapp
docker compose up -d
docker compose ps
```

<img width="1920" height="1030" alt="image" src="https://github.com/user-attachments/assets/4735c6f2-e907-4604-bcf8-c8b683cdc008" />

## G.5 Public ứng dụng bằng router/route trỏ về container (subdomain)

Trên Cloudflare Zero Trust:

1. **Networks → Tunnels** → chọn tunnel `myapp-tunnel`
2. Vào tab **Published application routes**
3. Chọn **Add a published application route**

Điền:

### Hostname
- **Subdomain:** `www`
- **Domain:** `luongvanhoc.io.vn`
- **Path:** để trống (public toàn bộ website)

### Service (Origin)
- **Type:** `HTTP`
- **URL:** `nginx:80`

Bấm **Save / Complete setup**.

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b1fa3eb-ae5e-482a-bd98-b8c1338608d5" />

## G.6 Kiểm tra URL đã public cho end-user

### 6.1 Kiểm tra website
Truy cập từ trình duyệt:

- `http://www.luongvanhoc.io.vn/`

Website hiển thị bình thường.


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0dcc69db-c46e-4735-addf-2576332d652b" />

### 6.2 Kiểm tra API qua Nginx
Thực hiện gọi API (qua web hoặc trực tiếp endpoint) và nhận phản hồi **200 OK** (Node-RED trả JSON “Hello from Node-RED”).

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d282604f-176b-47ae-ae4d-b0753190add0" />



















