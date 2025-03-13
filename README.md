# Workflow Tự Động Đăng Bài Lên WordPress Sử Dụng n8n

Repository này chứa hướng dẫn và workflow mẫu để tự động đăng bài lên website WordPress sử dụng công cụ tự động hóa n8n.

## n8n là gì?

n8n là một công cụ tự động hóa workflow mã nguồn mở, cho phép bạn kết nối các dịch vụ khác nhau và tự động hóa các tác vụ mà không cần viết code. n8n có giao diện trực quan dạng kéo thả, giúp bạn dễ dàng tạo các quy trình tự động.

## Workflow Tự Động Đăng Bài WordPress

### Yêu cầu:
- Đã cài đặt n8n (có thể cài đặt qua Docker, npm, hoặc n8n.cloud)
- Có website WordPress với REST API được bật
- Có thông tin xác thực WordPress (username/password hoặc Application Password)

### Các bước thực hiện:

1. **Cài đặt n8n**
   ```bash
   # Cài đặt qua npm
   npm install n8n -g
   
   # Hoặc sử dụng Docker
   docker run -it --rm \
     --name n8n \
     -p 5678:5678 \
     -v ~/.n8n:/home/node/.n8n \
     n8nio/n8n
   ```

2. **Truy cập n8n**
   - Mở trình duyệt và truy cập: http://localhost:5678
   - Đăng ký tài khoản hoặc đăng nhập

3. **Tạo Workflow Mới**
   - Nhấp vào "Create New Workflow"
   - Đặt tên cho workflow, ví dụ: "WordPress Auto Poster"

4. **Thiết lập Workflow**
   - Workflow này sẽ bao gồm các node sau:
     - Trigger node (Schedule, Webhook, hoặc RSS Feed)
     - Xử lý dữ liệu (Function, Text Manipulation)
     - WordPress node để đăng bài

## Chi Tiết Workflow

Xem file `wordpress-auto-post.json` trong repository này để import vào n8n của bạn.

### Mô tả các node:

1. **Trigger Node (RSS Feed)**
   - Theo dõi nguồn RSS để lấy nội dung mới
   - Cấu hình: URL của feed, khoảng thời gian kiểm tra

2. **Function Node**
   - Xử lý và định dạng dữ liệu từ RSS
   - Chuẩn bị tiêu đề, nội dung, và các thông tin khác

3. **WordPress Node**
   - Kết nối với WordPress site
   - Đăng bài với thông tin đã xử lý
   - Cấu hình: URL của WordPress site, thông tin xác thực, trạng thái bài đăng

## Hướng Dẫn Import Workflow

1. Tải file `wordpress-auto-post.json` từ repository này
2. Trong n8n, nhấp vào "Import from File"
3. Chọn file đã tải xuống
4. Cấu hình các thông tin xác thực và URL

## Tùy Chỉnh Workflow

Bạn có thể tùy chỉnh workflow này bằng cách:
- Thay đổi trigger (sử dụng Schedule thay vì RSS)
- Thêm các node xử lý nội dung (như AI Text Generation)
- Thêm các node thông báo (Email, Telegram, Slack)
- Thêm điều kiện đăng bài (IF node)

## Tài Liệu Tham Khảo

- [Tài liệu chính thức của n8n](https://docs.n8n.io/)
- [WordPress REST API Handbook](https://developer.wordpress.org/rest-api/)
- [n8n WordPress Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.wordpress/)

## Đóng Góp

Nếu bạn có cải tiến hoặc workflow mới, hãy tạo pull request hoặc issue!