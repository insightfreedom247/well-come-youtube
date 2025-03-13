# Hướng Dẫn Cài Đặt n8n

n8n là một công cụ tự động hóa workflow mạnh mẽ, cho phép bạn kết nối các dịch vụ khác nhau và tự động hóa các tác vụ mà không cần viết code. Dưới đây là các cách cài đặt n8n:

## 1. Cài đặt qua Docker (Khuyến nghị)

Docker là cách đơn giản nhất để chạy n8n mà không cần cài đặt nhiều dependencies.

```bash
# Chạy n8n với Docker
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n

# Chạy n8n với Docker Compose
# Tạo file docker-compose.yml với nội dung:
```

```yaml
version: '3'

services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    volumes:
      - ~/.n8n:/home/node/.n8n
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=user
      - N8N_BASIC_AUTH_PASSWORD=password
```

```bash
# Chạy với docker-compose
docker-compose up -d
```

## 2. Cài đặt qua npm

Nếu bạn đã có Node.js (v14 trở lên), bạn có thể cài đặt n8n qua npm:

```bash
# Cài đặt n8n globally
npm install n8n -g

# Chạy n8n
n8n start
```

## 3. Sử dụng n8n.cloud (Dịch vụ Hosted)

Nếu bạn không muốn tự cài đặt, bạn có thể sử dụng dịch vụ n8n.cloud:

1. Đăng ký tài khoản tại [n8n.cloud](https://www.n8n.cloud/)
2. Tạo một instance mới
3. Bắt đầu sử dụng ngay mà không cần cài đặt

## Truy cập n8n

Sau khi cài đặt, bạn có thể truy cập n8n qua trình duyệt:

- URL: `http://localhost:5678`
- Đăng ký tài khoản hoặc đăng nhập (nếu đã cấu hình)

## Cấu hình cơ bản

### Biến môi trường

Bạn có thể cấu hình n8n thông qua các biến môi trường:

```bash
# Bảo mật
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=user
N8N_BASIC_AUTH_PASSWORD=password

# Database
N8N_DB_TYPE=sqlite
N8N_DB_SQLITE_PATH=/home/node/.n8n/database.sqlite

# Hoặc sử dụng PostgreSQL
N8N_DB_TYPE=postgresdb
N8N_DB_POSTGRESDB_HOST=postgres
N8N_DB_POSTGRESDB_DATABASE=n8n
N8N_DB_POSTGRESDB_USER=postgres
N8N_DB_POSTGRESDB_PASSWORD=password
```

### Cài đặt SSL

Để bảo mật kết nối, bạn nên cấu hình SSL:

```bash
# Sử dụng proxy như Nginx hoặc cấu hình trực tiếp
N8N_PROTOCOL=https
N8N_SSL_KEY=/path/to/key.pem
N8N_SSL_CERT=/path/to/cert.pem
```

## Cập nhật n8n

### Docker

```bash
# Pull image mới nhất
docker pull n8nio/n8n

# Khởi động lại container
docker-compose down
docker-compose up -d
```

### npm

```bash
# Cập nhật n8n
npm update -g n8n
```

## Khắc phục sự cố

### Lỗi kết nối

Nếu bạn không thể kết nối đến n8n:
- Kiểm tra xem port 5678 đã được mở chưa
- Kiểm tra firewall
- Kiểm tra logs: `docker logs n8n`

### Lỗi database

Nếu gặp lỗi database:
- Kiểm tra quyền truy cập thư mục ~/.n8n
- Backup database trước khi cập nhật

## Tài liệu tham khảo

- [Tài liệu chính thức n8n](https://docs.n8n.io/)
- [GitHub repository](https://github.com/n8n-io/n8n)
- [Community forum](https://community.n8n.io/)