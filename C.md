# C. Cấu hình Docker Compose

# C-1. Tạo thư mục `~/myapp`
Chạy lệnh:
```bash
mkdir -p ~/myapp
```
<img width="1102" height="640" alt="image" src="https://github.com/user-attachments/assets/06d2432c-d971-4f28-b44e-a4690c0acff8" />


# C-2. Chuyển vào trong thư mục `~/myapp`
Chạy lệnh:
```bash
cd ~/myapp
```

# C-3. Tạo thư mục `./myweb`
Đảm bảo đang đứng trong `~/myapp`, sau đó chạy:
```bash
mkdir -p ./myweb
```
<img width="1105" height="640" alt="image" src="https://github.com/user-attachments/assets/989fd37f-ae42-4252-ba86-9eced1cebb04" />

# C-4. Tạo file `./myweb/index.html` (nội dung thông tin cá nhân)
Tạo và chỉnh sửa file:
```bash
nano ./myweb/index.html
```
<img width="1107" height="641" alt="image" src="https://github.com/user-attachments/assets/9681c351-b8fc-4690-9f0e-8c890ab522ee" />

Nhập nội dung HTML (Thông tin cá nhân):
```html
<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Thông tin cá nhân</title>
</head>
<body>
  <h1>Thông tin cá nhân</h1>
  <ul>
    <li>Họ và tên: Lương Văn Học</li>
    <li>MSSV: K225480106025</li>
    <li>Lớp: K58KTP</li>
    <li>Trường: Đại học Kỹ thuật Công nghiệp Thái Nguyên</li>
    <li>Email: luonghoc2604@gmail.com</li>
  </ul>
</body>
</html>
```

<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/8f9015db-c4f9-4528-983e-099191f87648" />

Lưu và thoát nano:
- Lưu: `Ctrl + O` → Enter
- Thoát: `Ctrl + X`

# C-5. Tạo file `docker-compose.yml` (Node-RED + Nginx)

## 1) Tạo các thư mục cần thiết
Trong thư mục `~/myapp`, tạo các thư mục chứa dữ liệu Node-RED và cấu hình Nginx:
```bash
cd ~/myapp
mkdir -p ./nodered ./nginx
```

<img width="1108" height="640" alt="image" src="https://github.com/user-attachments/assets/a4dd2ef1-350f-4415-960a-6d8f171e1e8c" />

## 2)  Tạo thư mục dữ liệu cho Node-RED và thư mục cấu hình cho Nginx
```bash
mkdir -p ./nodered ./nginx
```

<img width="1104" height="637" alt="image" src="https://github.com/user-attachments/assets/c17c5207-420c-4cdb-a04e-899f29659cd8" />


## 3)  Tạo file `./nginx/nginx.conf` để tránh lỗi mount
> Ở bước C-6 sẽ chỉnh cấu hình nginx chi tiết.  
> Tuy nhiên, để đảm bảo `docker compose` không lỗi vì thiếu file mount, tạo file tối thiểu trước.

```bash
nano ./nginx/nginx.conf
```

Dán cấu hình tối thiểu:
```nginx
events {}

http {
  server {
    listen 80;
    server_name localhost;

    location / {
      root /myweb;
      index index.html;
      try_files $uri $uri/ /index.html;
    }
  }
}
```

Lưu và thoát:
- Lưu: `Ctrl + O` → Enter
- Thoát: `Ctrl + X`


## 4) Tạo file `docker-compose.yml`
Tạo và chỉnh sửa:
```bash
nano docker-compose.yml
```
<img width="1104" height="639" alt="image" src="https://github.com/user-attachments/assets/ba3226da-9581-4116-a50e-325deae90fa9" />

Nội dung file `docker-compose.yml`:
```yaml
services:
  nodered:
    image: nodered/node-red:latest
    container_name: nodered
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data
    restart: unless-stopped

  nginx:
    image: nginx:latest
    container_name: nginx
    ports:
      - "80:80"
    volumes:
      - ./myweb:/myweb:ro
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - nodered
    restart: unless-stopped
```
<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/f889f4c2-2463-4ef3-97b6-2c0ecbb54865" />

Lưu và thoát:
- Lưu: `Ctrl + O` → Enter
- Thoát: `Ctrl + X`




# C-6. Cấu hình Nginx Reverse Proxy (`./nginx/nginx.conf`) 

## Bước 1 — Mở file cấu hình Nginx
```bash
cd ~/myapp
nano ./nginx/nginx.conf
```


## Bước 2 — Cấu hình `nginx.conf`

```nginx
events {}

http {
  server {
    # (1) Web server cổng 80
    listen 80;

    # (2) server_name là sub-domain 
    server_name app.luongvanhoc.io.vn;

    # (3) location / trỏ tới root là /myweb (web tĩnh)
    location / {
      root /myweb;
      index index.html index.htm;
      try_files $uri $uri/ /index.html;
    }

    # (4) location /api proxy_pass sang Node-RED (http_in)
    # Lưu ý quan trọng: KHÔNG có dấu "/" ở cuối proxy_pass để giữ nguyên đường dẫn /api/...
    location /api/ {
      proxy_pass http://nodered:1880;

      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
    }
  }
}
```

Lưu và thoát nano:
- Lưu: `Ctrl + O` → Enter
- Thoát: `Ctrl + X`

---

## Bước 3 — Khởi động/restart Docker Compose để áp dụng cấu hình
Nếu các container chưa chạy:
```bash
docker compose up -d
```

Kiểm tra container:
```bash
docker compose ps
```
<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/f3df77da-cf3d-439b-84e3-4ce67fa896ae" />


## Bước 4 — Tạo API trong Node-RED để test proxy `/api`
Mở Node-RED trên trình duyệt:
- `http://192.168.1.8:1880/`

Tạo flow gồm 3 node:
1. **http in**
   - Method: `GET`
   - URL: `/api/hello`
2. **function** (nội dung):
   ```js
   msg.payload = { ok: true, msg: "Hello from Node-RED" };
   return msg;
   ```
3. **http response**

Nối: `http in` → `function` → `http response`, sau đó bấm **Deploy**.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8b47733f-0ef1-4f0a-9b2a-66ae3916a0a0" />


## Bước 5 — Kiểm tra kết quả
### 5.1. Kiểm tra web tĩnh (location `/`)
```bash
curl -I http://localhost/
curl http://localhost/ | head -n 20
```

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/ab56cb8b-f579-4719-b2af-915810a16dec" />

### 5.2. Kiểm tra proxy sang Node-RED (location `/api`)
```bash
curl http://localhost/api/hello
```
<img width="893" height="389" alt="image" src="https://github.com/user-attachments/assets/e7625dca-dba8-4b72-9dbf-ce14f29b6486" />


# C-7. Bắt buộc đăng nhập Node-RED (Edit `./nodered/settings.js`)


## 1) Chạy docker compose lần đầu để Node-RED tự sinh cấu hình trong `./nodered`
Chuyển vào thư mục dự án và chạy compose:

```bash
cd ~/myapp
docker compose up -d
```

Kiểm tra các container đang chạy:

```bash
docker compose ps
```

Kiểm tra Node-RED đã tự sinh dữ liệu và file cấu hình trong thư mục `./nodered`:

```bash
ls -la ./nodered | head
ls -la ./nodered/settings.js
```

<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/35e68901-8344-4dc9-8d9f-7cf613e2719f" />

## 2) Tạo mật khẩu dạng hash (bcrypt) để dùng trong `settings.js`
Node-RED yêu cầu lưu mật khẩu ở dạng hash. Tạo hash bằng cách chạy lệnh sau:

```bash
docker exec -it nodered node -e "console.log(require('bcryptjs').hashSync(process.argv[1], 8));" '@12345'
```
Sau khi chạy, terminal sẽ in ra chuỗi hash `$2b$08$FBCqY.tD8km4rgWhSquBFe0Q9CybgdAQosQ5HQdFj0NH8H2qXAD5.`.  
**Copy chuỗi hash này** để dán vào `settings.js`.

<img width="1980" height="1080" alt="image" src="https://github.com/user-attachments/assets/5693c6ae-b262-42ac-a036-6fdcc1105a3c" />


## 3) Edit `./nodered/settings.js` để bật đăng nhập (adminAuth)
Mở file:

```bash
nano ./nodered/settings.js
```

Trong nano, tìm `adminAuth`:
- Bấm `Ctrl + W`
- Gõ `adminAuth` rồi Enter

Tại block `adminAuth`, tiến hành:
- **Bỏ comment** (xoá dấu `//` ở đầu các dòng của block `adminAuth` nếu đang bị comment)
- Điền `username` và dán `password` (hash bcrypt) vừa tạo

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/89399d3e-f9c0-4a41-bef2-526f8433a0ff" />

Lưu và thoát:
- Lưu: `Ctrl + O` → Enter
- Thoát: `Ctrl + X`


## 4) Restart Node-RED để áp dụng cấu hình
```bash
docker compose restart nodered
```

## 5) Kiểm tra kết quả
Mở trình duyệt:
- `http://192.168.1.8:1880/`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/df2e94de-4da3-41c1-8fec-2de2590e0628" />





