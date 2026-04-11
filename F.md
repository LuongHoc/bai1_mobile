# F. Gỡ lỗi (Debug)

## F.1) Nếu có lỗi xảy ra trong quá trình triển khai `docker compose up -d`

### 1) Chạy triển khai
```bash
cd ~/myapp
docker compose up -d
```
<img width="1105" height="639" alt="image" src="https://github.com/user-attachments/assets/9a11ee4e-5e0c-45bf-b24d-128f093753be" />

### 2) Kiểm tra nhanh container nào đang chạy / bị lỗi
```bash
docker compose ps
```
<img width="1103" height="644" alt="image" src="https://github.com/user-attachments/assets/8b58186b-c9c8-4617-a7fe-8f355ed2c88a" />

### 3) Xem log của container/service để tìm lỗi

```bash
docker logs nginx
docker logs myapi
```

<img width="1107" height="637" alt="image" src="https://github.com/user-attachments/assets/bf85f692-e098-4839-8b58-e32a53566ec5" />

### 4) Sửa cấu hình rồi chạy lại
- Sửa các file liên quan (`docker-compose.yml`, `./nginx/nginx.conf`, `./nodered/settings.js`, `./myapi/app.py`, …)
- Áp dụng lại:

Nếu chỉ sửa file cấu hình mount (nginx.conf, settings.js, …) thì restart service:
```bash
docker compose restart nginx
docker compose restart nodered
docker compose restart myapi
```

Nếu có sửa Dockerfile/requirements/code của `myapi` (cần build lại image):
```bash
docker compose up -d --build myapi
```

Sau đó kiểm tra lại:
```bash
docker compose ps
```


## F.2) Thêm healthcheck cho `myapi` trong file `docker-compose.yml`

### 1) Thêm cấu hình healthcheck
Mở file:
```bash
nano ~/myapp/docker-compose.yml
```

Thêm vào service `myapi`:

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:9630"]
  interval: 10s
  timeout: 5s
  retries: 5
  start_period: 10s
```

<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/8f43fe41-eb47-4529-9ac9-5d7b7b0a847f" />


### 2) Áp dụng cấu hình
```bash
docker compose up -d
docker compose ps
```

## F.3) Giới hạn resource cho một service + quan sát RAM bằng `docker compose stats`

### 1) Giới hạn RAM cho 1 service 
Mở file:
```bash
nano ~/myapp/docker-compose.yml
```

Thêm vào service cần giới hạn:

```yaml
deploy:
  resources:
    limits:
      memory: 512M
```
<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/dbdd2b46-c254-4323-89da-8c03e5699b95" />

Áp dụng:
```bash
docker compose up -d
```

### 2) Quan sát lượng RAM sử dụng của mỗi service
Chạy:
```bash
docker compose stats
```
<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/f3876012-e9b8-4b4f-ae5d-939341f805b7" />

Thoát:
- `Ctrl + C`











