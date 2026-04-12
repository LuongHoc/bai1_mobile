# D. Bonus

## D-1. Tạo thư mục `./myapi`
```bash
cd ~/myapp
mkdir -p ./myapi
```

<img width="1103" height="642" alt="image" src="https://github.com/user-attachments/assets/c1ab97a3-70f1-4834-b289-2bb978f99b35" />


## D.2. Tạo file `./myapi/app.py` (Flask API “funny”/demo)
Tạo file:
```bash
nano ./myapi/app.py
```
<img width="1107" height="639" alt="image" src="https://github.com/user-attachments/assets/ba0eb800-8ef9-49c1-a4e2-4d3fb23ab435" />

Dán nội dung:

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/tinh-vat', methods=['GET'])
def tinh_vat():
    # Lấy giá trị từ tham số "tien" trên URL
    tien_input = request.args.get('tien')

    # Kiểm tra xem người dùng có nhập tiền hay không
    if tien_input is None:
        return jsonify({"error": "Vui lòng cung cấp tham số 'tien'"}), 400

    try:
        # Chuyển đổi sang kiểu số thực và tính toán
        so_tien = float(tien_input)
        ket_qua = so_tien * 1.1

        return jsonify({
            "so_tien_goc": so_tien,
            "thue_vat": "10%",
            "tong_cong": ket_qua
        })
    except ValueError:
        # Trả về lỗi nếu đầu vào không phải là số
        return jsonify({"error": "Giá trị 'tien' phải là một con số hợp lệ"}), 400

if __name__ == '__main__':
    # Chạy ứng dụng tại cổng 9630
    app.run(host='0.0.0.0', port=9630)
```

<img width="1103" height="639" alt="image" src="https://github.com/user-attachments/assets/ff3e2734-611b-4da5-a7f5-31ae3d00599f" />

Lưu/thoát:
- Lưu: `Ctrl + O` → Enter
- Thoát: `Ctrl + X`

## D.3. Tạo file `./myapi/requirements.txt`
```bash
nano ./myapi/requirements.txt
```

Nội dung:
```txt
flask
```

<img width="1104" height="642" alt="image" src="https://github.com/user-attachments/assets/d89c2830-b68a-43c8-9ae4-9ea772e14d58" />

## D.4. Tạo file `./myapi/Dockerfile` (Python 3.9 slim)
```bash
nano ./myapi/Dockerfile
```
<img width="1106" height="637" alt="image" src="https://github.com/user-attachments/assets/277b2c6c-f80e-4048-b758-556ef39a7cd6" />

Nội dung:
```dockerfile
# Sử dụng phiên bản Python nhẹ để giảm dung lượng image
FROM python:3.9-slim

# Thiết lập thư mục làm việc bên trong container
WORKDIR /app

# Sao chép file requirements vào và cài đặt thư viện
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Sao chép toàn bộ mã nguồn vào container
COPY . .

# Thông báo container sẽ chạy ở cổng 9630
EXPOSE 9630

# Lệnh khởi chạy ứng dụng
CMD ["python", "app.py"]
```


## D.5. Sửa `docker-compose.yml` để chạy service `myapi`
Mở file:
```bash
nano ~/myapp/docker-compose.yml
```
<img width="1105" height="638" alt="image" src="https://github.com/user-attachments/assets/835a14c1-c7ec-4fe1-9774-2bcd429b4412" />

Thêm service `myapi` (đảm bảo indent YAML đúng, `myapi` phải cùng cấp với `nginx`, `nodered`):

```yaml
  myapi:
    build:
      context: ./myapi
      dockerfile: Dockerfile
    container_name: myapi
    ports:
      - "9630:9630"
    restart: unless-stopped
```
<img width="1105" height="637" alt="image" src="https://github.com/user-attachments/assets/2c70c036-c97f-450d-8195-d9a67f84e75b" />


Kiểm tra cấu hình compose:
```bash
cd ~/myapp
docker compose config
```

Build + chạy lại:
```bash
docker compose up -d --build
docker compose ps
```

### Test `myapi` trực tiếp (không qua Nginx)
```bash
curl "http://localhost:9630/tinh-vat?tien=100"
```

Kỳ vọng trả JSON:
```json
{"so_tien_goc":100.0,"thue_vat":"10%","tong_cong":110.0}
```
<img width="1107" height="643" alt="image" src="https://github.com/user-attachments/assets/b4df0940-5d80-4470-b03e-21f026410410" />


## D.6. Sửa `nginx/nginx.conf` để `/api` trỏ tới `myapi` cổng 9630
Mở file:
```bash
nano ~/myapp/nginx/nginx.conf
```

Sửa/Thay block `location /api` (Bonus) thành:

```nginx
location /api {
  proxy_pass http://myapi:9630/tinh-vat;
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto $scheme;
}
```
<img width="1105" height="641" alt="image" src="https://github.com/user-attachments/assets/f4927aa3-8f12-4efd-b5f0-58bc3fb306c9" />


Restart nginx:
```bash
docker compose restart nginx
```

## Kiểm thử cuối cùng (qua Nginx Reverse Proxy)
Gọi API thông qua Nginx (port 80):
```bash
curl -i "http://localhost/api?tien=100"
```

<img width="1109" height="639" alt="image" src="https://github.com/user-attachments/assets/a08f0ba5-1230-4525-b511-79406fd8ecb9" />

