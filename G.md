




# G. Triển khai ứng dụng đến End-user (Cloudflare Tunnel + Docker Compose)

> Ngày thực hiện: 2026-04-11  
> Mục tiêu: Public website + API từ server chạy Docker ra Internet thông qua **Cloudflare Tunnel**, người dùng truy cập bằng **subdomain/domain**.

---

## 1) Tạo Tunnel trên Cloudflare (chọn triển khai Docker)

1. Đăng nhập Cloudflare Zero Trust: `dash.cloudflare.com` → **Zero Trust**.
2. Vào **Networks** → **Tunnels** → **Create a tunnel**.
3. Chọn loại **Cloudflared**.
4. Đặt tên tunnel (ví dụ: `myapp-tunnel`) → **Create**.
5. Ở bước **Install and run a connector**, chọn **Docker** và copy **Tunnel Token** (chuỗi bắt đầu bằng `eyJ...`).

> Lưu ý: Token dùng cho tunnel là **Tunnel Token**, không phải API Token.

---

## 2) Convert lệnh `docker run ...` sang `docker compose`

Thay vì chạy trực tiếp `docker run cloudflare/cloudflared ...`, ta đưa cấu hình vào `docker-compose.yml` để quản lý cùng các service (nginx, node-red, myapi,...).

---

## 3) Khai báo kết quả convert vào `docker-compose.yml`

### 3.1 Tạo file `.env` chứa token
Tạo/ sửa file `.env` trong thư mục dự án:

```env
CLOUDFLARE_TUNNEL_TOKEN=eyJ...  # chỉ dán token, KHÔNG kèm '--token'
```

> Nếu lỡ dán kiểu `--token eyJ...` sẽ bị lỗi: `Provided Tunnel token is not valid.`

### 3.2 Khai báo service `cloudflared` trong `docker-compose.yml`
Ví dụ (đặt cùng network với nginx):

```yaml
services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    command: tunnel --no-autoupdate run --token ${CLOUDFLARE_TUNNEL_TOKEN}
    restart: unless-stopped

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: unless-stopped
    ports:
      - "80:80"
```

> Khi publish qua tunnel, Cloudflare sẽ route về service nội bộ theo tên container/service (ví dụ `nginx:80`).

---

## 4) Chạy lại Docker Compose

Trong thư mục dự án:

```bash
cd ~/myapp
docker compose up -d
docker compose ps
```

Kiểm tra log tunnel:

```bash
docker logs -f cloudflared
```

**Dấu hiệu OK** (tunnel đã kết nối): có các dòng kiểu:
- `Starting tunnel tunnelID=...`
- `Registered tunnel connection ...`

---

## 5) Public ứng dụng bằng cách thêm router (Published application route)

Mục tiêu: public URL dạng subdomain (ví dụ `www.luongvanhoc.io.vn`) trỏ vào container đang chạy trong docker.

1. Cloudflare Zero Trust → **Networks** → **Tunnels** → chọn tunnel `myapp-tunnel`.
2. Chọn tab **Published application routes** → **Add a route** (hoặc “Add a published application route”).

Điền cấu hình:

### 5.1 Hostname
- **Subdomain**: `www` (hoặc `app`)
- **Domain**: `luongvanhoc.io.vn`
- **Path**: để trống (match tất cả đường dẫn)

### 5.2 Service (Origin)
- **Type**: `HTTP`
- **URL**: `nginx:80` (hoặc `http://nginx:80` tùy giao diện)

Bấm **Save/Create**.

> Lưu ý: `nginx:80` là service nội bộ trong docker network (cloudflared sẽ forward traffic tới nginx).

---

## 6) Kiểm tra URL đã public cho end-user

### 6.1 Kiểm tra web
Mở trình duyệt và truy cập:

- `http://www.luongvanhoc.io.vn/`  
  (hoặc `http://app.luongvanhoc.io.vn/` nếu dùng subdomain `app`)

Trang web hiển thị bình thường (nginx).

### 6.2 Kiểm tra API qua Nginx (reverse proxy)
Trên web có mục test API và trả về **Status: 200**, ví dụ:

- `GET /api/api/hello`
- JSON trả về:
  ```json
  {"ok":true,"msg":"Hello from Node-RED"}
  ```

=> Kết luận: Website + API đã public thành công qua Cloudflare Tunnel.

---

## Troubleshooting nhanh

### A) Lỗi `Provided Tunnel token is not valid.`
Nguyên nhân thường gặp:
- Dán token sai (dán cả `--token`)
- Token copy thiếu ký tự

Cách sửa:
- `.env` phải là:
  ```env
  CLOUDFLARE_TUNNEL_TOKEN=eyJ...
  ```
- Chạy lại:
  ```bash
  docker compose down
  docker compose up -d
  docker logs -f cloudflared
  ```

### B) Vào domain bị 502/Bad Gateway
- Kiểm tra `Service URL` trên Cloudflare route đã đúng `nginx:80` chưa.
- Xem log nginx:
  ```bash
  docker logs nginx --tail=100
  ```

---

## Kết quả đạt được
- Tunnel `myapp-tunnel` **HEALTHY/Connected**
- URL public hoạt động cho end-user: `www.luongvanhoc.io.vn`
- API qua nginx trả về **200 OK**






<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/55c33b7e-75e6-4fbe-ba29-91f46e8c7ce3" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/139cdf4f-a884-4e90-87c8-f0b2fdd4a51a" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7e250934-2e45-4b12-98e0-74b84eb7be4a" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf10ce04-2b22-4133-ae81-edb97f56b4cf" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a66d16be-1e11-4804-a27c-05a1f330ba32" />


<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/2bed2bb4-f80d-494e-b7fb-471e454f89b0" />


<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/3bed7063-9043-4697-8e23-9ec38b4e4cca" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/99e17e1a-075d-48e1-b342-5f34e093d67f" />










