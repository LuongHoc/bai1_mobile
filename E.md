# E. Triển khai (level test) ứng dụng

## E.1) Chuyển vào trong thư mục dự án
```bash
cd ~/myapp
pwd
```

<img width="1105" height="612" alt="image" src="https://github.com/user-attachments/assets/ecc90b22-95b5-4b38-836c-03fbbdee2af6" />


## E.2) Chạy Docker Compose (run tất cả services trong `docker-compose.yml`)
Chạy lệnh:
```bash
docker compose up -d
```

<img width="1106" height="614" alt="image" src="https://github.com/user-attachments/assets/6b1cda63-1d0f-4782-9255-b4aa6994e6b1" />

## E.3) Kiểm tra các container đang chạy (phát hiện restart)
Kiểm tra trạng thái:
```bash
docker compose ps
```

<img width="1109" height="613" alt="image" src="https://github.com/user-attachments/assets/f2296a63-af85-4c1c-8c3d-6d7222c57f5e" />


## E.4) Kiểm thử các service đang chạy độc lập theo IP và port

### 1) Xem IP của Ubuntu
```bash
ip -4 addr
```

<img width="1109" height="611" alt="image" src="https://github.com/user-attachments/assets/fbd56bcf-87fc-4d0a-b539-fe87d55f3ef2" />

### 2) Kiểm thử Nginx (web server port 80)
```bash
curl -I http://localhost/
```
Kỳ vọng: `HTTP/1.1 200 OK`.

<img width="1106" height="613" alt="image" src="https://github.com/user-attachments/assets/a62e618b-838a-4c75-99e5-6c5bbd214de3" />

Trên Windows mở trình duyệt:
- `http://192.168.1.8/`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7232ebfc-dc88-43d6-a503-7103b719390c" />


### 3) Kiểm thử Node‑RED (port 1880)
Trên Ubuntu:
```bash
curl -I http://localhost:1880/
```
Kỳ vọng: `HTTP/1.1 200 OK` (hoặc redirect nếu có cấu hình login).

<img width="1106" height="612" alt="image" src="https://github.com/user-attachments/assets/7fa7e9c9-2dee-48a9-89bf-62d8c197b6ad" />


Trên Windows mở trình duyệt:
- `http://192.168.1.8:1880/`
- 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/76e8d62f-b092-4bf8-9999-100eb1d4e64a" />


## E.5) Sử dụng Node‑RED tạo API GET đơn giản (http_in → function → http_response)

### 1) Tạo Flow trong Node‑RED
Mở Node‑RED:  
`http://192.168.1.8:1880/`

Tạo 3 node và nối theo thứ tự:

1. **http in**
   - Method: `GET`
   - URL: `/api/hello`

2. **function** (ví dụ trả JSON đơn giản)
   ```js
   msg.payload = { ok: true, msg: "Hello from Node-RED" };
   return msg;
   ```

3. **http response**

Bấm **Deploy** để lưu flow.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/21a4fbef-b21a-44eb-b2bb-04b7c27236b1" />


### 2) Test API trực tiếp qua Node‑RED
```bash
curl -i http://localhost:1880/api/hello
```
Kỳ vọng: `HTTP/1.1 200 OK` và JSON `{"ok":true,"msg":"Hello from Node-RED"}`.

<img width="1102" height="612" alt="image" src="https://github.com/user-attachments/assets/2c71d59e-ecdf-481d-b8af-cc4f3093f615" />


### 3) Cấu hình Nginx `/api/` reverse proxy sang Node‑RED
Mở file:
```bash
nano ~/myapp/nginx/nginx.conf
```

Thêm/sửa block trong `server { ... }`:
```nginx
location /api/ {
  proxy_pass http://nodered:1880/;
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto $scheme;
}
```
<img width="1104" height="609" alt="image" src="https://github.com/user-attachments/assets/b80ad850-79bf-4949-a15d-d974df583a42" />

Restart Nginx:
```bash
docker compose restart nginx
```

### 4) Test API qua Nginx Reverse Proxy
Do Node‑RED endpoint đang là `/api/hello`, và Nginx prefix cũng là `/api/`, nên URL test sẽ là:

```bash
curl -i http://localhost/api/api/hello
```

<img width="1111" height="614" alt="image" src="https://github.com/user-attachments/assets/717c8b06-af72-4f2a-8f37-6e3bfc33dd9d" />

## E.6) Sửa `./myweb/index.html` để gọi API đã khai báo proxy_pass (qua Nginx)

Mở file:
```bash
nano ~/myapp/myweb/index.html
```

Nội dung ví dụ hoàn chỉnh (HTML + JS gọi API qua Nginx):

```html
<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Bài 01 - Web + API</title>
  <style>
    body { font-family: Arial, Helvetica, sans-serif; margin: 24px; }
    input, button { padding: 6px 10px; font-size: 14px; }
    pre { background: #f5f5f5; padding: 12px; border: 1px solid #ddd; white-space: pre-wrap; }
  </style>
</head>

<body>
  <h1>Thông tin cá nhân</h1>
  <ul>
    <li>Họ và tên: Lương Văn Học</li>
    <li>MSSV: K225480106025</li>
    <li>Lớp: 58KTPM</li>
    <li>Môn: Phát triển ứng dụng với mã nguồn mở (TEE0421)</li>
  </ul>

  <hr />

  <h2>Test API qua Nginx</h2>
  <button id="btn">Gọi API</button>
  <pre id="out">Chưa gọi</pre>

  <script>
    const out = document.getElementById("out");
    const btn = document.getElementById("btn");

    btn.addEventListener("click", async () => {
      // Với cấu hình E-5 hiện tại:
      // Nginx: /api/ -> proxy sang Node-RED /
      // Node-RED endpoint: /api/hello
      // => URL gọi qua Nginx: /api/api/hello
      const url = "/api/api/hello";

      out.textContent = "Đang gọi: " + url;

      try {
        const res = await fetch(url, { method: "GET" });
        const text = await res.text();
        out.textContent = `GET ${url}\nStatus: ${res.status}\n\n${text}`;
      } catch (err) {
        out.textContent = "Lỗi khi gọi API: " + err;
      }
    });
  </script>
</body>
</html>
```

<img width="1103" height="609" alt="image" src="https://github.com/user-attachments/assets/0e3435f4-538f-42a1-ba3d-fa3357c35a1f" />


### Kiểm thử trên trình duyệt (End-user)
Trên Windows mở:
- `http://192.168.1.8/`

Bấm **Gọi API** → kỳ vọng:
- hiển thị `GET /api/api/hello`
- `Status: 200`
- JSON: `{"ok":true,"msg":"Hello from Node-RED"}`

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b746f1f5-2e3b-4779-b173-3ea43f264d2f" />

