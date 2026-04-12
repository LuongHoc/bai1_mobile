# G. Triển khai ứng dụng đến End-user (Cloudflare Tunnel + Docker Compose)

## G.1 Tạo Cloudflare Tunnel (triển khai kiểu Docker)

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


## G.2 Convert lệnh `docker run` sang `docker compose`

Cloudflare cung cấp lệnh mẫu dạng:

```bash
docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token eyJhIjoiZWNhNDdhZWZiNmMyZWM3MWVkMWY5ZmI3NDZjZmQwNzMiLCJ0IjoiOTAyMDUxMzQtMTVlNi00ZWYwLTkxMzUtNGVjODhiZDQ4YWQwIiwicyI6Ik1XRmhOemxpWTJFdFpHWTFNeTAwWXpJekxXRTVOamN0WW1SaE5UZG1PVGRoWWpZMCJ9
```

Thay vì chạy trực tiếp, ta đưa cấu hình này vào `docker-compose.yml` để quản lý cùng các container khác (nginx, nodered, myapi,…).


## G.3 Khai báo cloudflared vào `docker-compose.yml`

### G.3.1 Tạo file `.env` để chứa token

Trong thư mục dự án, tạo file `.env`:

```bash
cd ~/myapp
nano .env
```
<img width="1105" height="643" alt="image" src="https://github.com/user-attachments/assets/e76cb4ef-bcb8-4b03-9142-507f1a3e8d2d" />

<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/ef9a5004-1d2f-459a-8460-fef5cf9f6f3e" />


### G.3.2 Thêm service `cloudflared` vào `docker-compose.yml`

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



## G.4 Chạy lại Docker Compose

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

### G.6.1 Kiểm tra website
Truy cập từ trình duyệt:

- `http://www.luongvanhoc.io.vn/`

Website hiển thị bình thường.


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0dcc69db-c46e-4735-addf-2576332d652b" />

### G.6.2 Kiểm tra API qua Nginx
Thực hiện gọi API (qua web hoặc trực tiếp endpoint) và nhận phản hồi **200 OK** (ví dụ Node-RED trả JSON “Hello from Node-RED”).

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d282604f-176b-47ae-ae4d-b0753190add0" />


# G. Câu hỏi về bài làm 


## 1) Tại sao phải dùng Nginx làm Reverse Proxy mà không trỏ thẳng Tunnel vào Node-RED?

Trong bài này hệ thống gồm **web tĩnh** (`./myweb/index.html`) và **API** (Node-RED và/hoặc myapi). Vì vậy Cloudflare Tunnel nên trỏ vào **Nginx** để:

- **Serve web tĩnh đúng yêu cầu bài**: `location /` trỏ tới thư mục `/myweb` (mount từ `./myweb`).

- **Định tuyến theo path**: `location /api` dùng `proxy_pass` để chuyển tiếp request tới Node-RED (hoặc myapi).

- **Một domain/subdomain cho nhiều dịch vụ**: người dùng chỉ truy cập 1 URL, Nginx đứng trước để chia route vào các service phía sau.

=> Tunnel trỏ vào `nginx:80` là đúng mô hình và đúng yêu cầu triển khai end-user.



## 2) Sự khác biệt giữa việc Mount file và Mount thư mục trong Docker là gì?

- **Mount file**: ánh xạ *một file* trên host vào *một file* trong container.

  Ví dụ: `./nginx/nginx.conf:/etc/nginx/nginx.conf`
  
  → container chỉ nhận đúng file cấu hình đó.

- **Mount thư mục**: ánh xạ *cả thư mục* trên host vào container.
  
  Ví dụ: `./myweb:/myweb`
  
  → container thấy toàn bộ các file trong thư mục, phù hợp cho web tĩnh (HTML/CSS/JS).



## 3) Nếu thay đổi file `index.html` ở máy Ubuntu, nội dung trên web có thay đổi ngay không? Tại sao?

**Thông thường có (gần như ngay)** nếu `index.html` nằm trong thư mục đang được bind mount:

- Host: `./myweb/index.html`

- Container Nginx: `/myweb/index.html`

Vì Nginx đọc file trực tiếp từ `/myweb` (thực chất là thư mục trên host qua mount), nên sửa file trên Ubuntu thì nội dung web thay đổi ngay.

Trường hợp không thấy đổi thường do **cache trình duyệt** (cần refresh mạnh Ctrl+F5 hoặc mở tab ẩn danh).


## 4) `docker-compose.yml` có `restart: always` hoặc `restart: unless-stopped` để làm gì?

Đây là **restart policy** của Docker giúp container tự chạy lại khi:

- Container bị crash

- Docker daemon bị restart / máy reboot

Khác nhau:
- `restart: always`: luôn cố gắng chạy lại (phù hợp service quan trọng như nginx).

- `restart: unless-stopped`: tự chạy lại **trừ khi** người dùng đã stop thủ công trước đó.


## 5) Cách khai báo để tất cả services dùng chung 1 network? Lợi ích? Sửa docker-compose

### Lợi ích
- Các service gọi nhau bằng **tên service** thay vì IP (ví dụ `proxy_pass http://myapi:9630`).

- Cloudflared route được tới origin nội bộ kiểu `nginx:80`.

- Traffic nội bộ rõ ràng, dễ quản lý và ổn định.

### Ví dụ sửa `docker-compose.yml` (dùng chung network `appnet`)
```yaml
services:
  myapi:
    build: ./myapi
    container_name: myapi
    restart: always
    networks: [appnet]

  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    restart: unless-stopped
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data
    networks: [appnet]

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb:ro
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    networks: [appnet]

  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    restart: unless-stopped
    command: tunnel --no-autoupdate run --token ${CLOUDFLARE_TUNNEL_TOKEN}
    networks: [appnet]

networks:
  appnet:
    driver: bridge
```


## 6) Đưa Cloudflare Token vào `.env` + thêm `.env` vào `.gitignore. Vì sao quan trọng?

### Cách làm
Tạo `.env` (cùng thư mục `docker-compose.yml`):
```env
CLOUDFLARE_TUNNEL_TOKEN=eyJ...
```

Trong `docker-compose.yml`:
```yaml
command: tunnel --no-autoupdate run --token ${CLOUDFLARE_TUNNEL_TOKEN}
```

Thêm vào `.gitignore`:
```gitignore
.env
```

### Vì sao quan trọng về bảo mật?
- Tunnel token là **secret**. Nếu push lên GitHub (đặc biệt repo public), người khác có thể dùng token để chạy connector trái phép, gây rủi ro truy cập/chiếm quyền điều hướng tunnel.

- Nguyên tắc quan trọng: **không commit secrets vào mã nguồn**.


## 7) Tại sao nên thêm hậu tố `:ro` khi mount file cấu hình Nginx?

`:ro` = **read-only** (chỉ đọc).

- Ngăn container sửa file cấu hình trên host.

- Tăng an toàn nếu container bị xâm nhập (khó bị chỉnh nginx.conf để chuyển hướng, mở đường dẫn nguy hiểm…).

- Đúng best practice “least privilege”.

Ví dụ:
- `./nginx/nginx.conf:/etc/nginx/nginx.conf:ro`

- `./myweb:/myweb:ro`


## 8) Khi dùng Cloudflare Tunnel: có cần thiết phải mở cổng cho các service nữa không?

**Không cần mở cổng public ra Internet** để end-user truy cập, vì:

- Cloudflared tạo kết nối **outbound** từ server tới Cloudflare.

- End-user truy cập domain → Cloudflare → Tunnel → origin nội bộ (ví dụ `nginx:80`).

Tuy nhiên:
- Trong quá trình **level test** theo đề (truy cập `ip_ubuntu:1880`, `ip_ubuntu:9630`) thì việc mở cổng (UFW allow) giúp test trực tiếp.

- Khi đã dùng Tunnel để public end-user thì việc mở port public không còn là điều kiện bắt buộc.


















