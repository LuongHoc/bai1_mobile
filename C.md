# C. Cấu hình Docker Compose

# C-1) Tạo thư mục `~/myapp`
Chạy lệnh:
```bash
mkdir -p ~/myapp
```
<img width="1102" height="640" alt="image" src="https://github.com/user-attachments/assets/06d2432c-d971-4f28-b44e-a4690c0acff8" />


## C-2) Chuyển vào trong thư mục `~/myapp`
Chạy lệnh:
```bash
cd ~/myapp
```

## C-3) Tạo thư mục `./myweb`
Đảm bảo đang đứng trong `~/myapp`, sau đó chạy:
```bash
mkdir -p ./myweb
```
<img width="1105" height="640" alt="image" src="https://github.com/user-attachments/assets/989fd37f-ae42-4252-ba86-9eced1cebb04" />

## C-4) Tạo file `./myweb/index.html` (nội dung thông tin cá nhân)
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















