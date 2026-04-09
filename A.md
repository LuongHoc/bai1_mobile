# A. Đăng ký tên miền xịn cho cá nhân:
## 1. Đăng kí domain 
Link đăng kí domain: https://tenten.vn/

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

