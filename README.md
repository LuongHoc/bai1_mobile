# Họ và tên: Lương Văn Học  - MSSV:K225480106025
# Lớp: K58KTP
# Môn: Phát triển ứng dụng với mã nguồn mở-TEE0421

# 📋 Báo Cáo Bài Tập 01 - Phát Triển Ứng Dụng Với Mã Nguồn Mở



## 📁 Mục Lục

| Phần | Nội dung | Link |
|---|---|---|
| **A** | Đăng ký tên miền & cấu hình Cloudflare | [A.md](./A.md) |
| **B** | Cài đặt Ubuntu 24.04.4 LTS + Docker + SSH + UFW | [B.md](./B.md) |
| **C** | Cấu hình Docker Compose (Nginx + Node-RED + Web) | [C.md](./C.md) |
| **D** | Bonus: Flask API (myapi) + Dockerfile + Nginx proxy | [D.md](./D.md) |
| **E** | Triển khai level test + kiểm thử các service | [E.md](./E.md) |
| **F** | Gỡ lỗi + healthcheck + giới hạn tài nguyên | [F.md](./F.md) |
| **G** | Public qua Cloudflare Tunnel + Q&A | [G.md](./G.md) |



## 🗂️ Cấu trúc thư mục dự án (tham khảo)

```text
myapp/
├── docker-compose.yml
├── .env                
├── .gitignore
├── nginx/
│   └── nginx.conf
├── myweb/
│   └── index.html
└── nodered/
    ├── settings.js
    └── (các file tự sinh khác)
```

