
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

