# Workflow Tự Động Đăng Bài Lên WordPress Sử Dụng n8n

Repository này chứa hướng dẫn và workflow mẫu để tự động đăng bài lên website WordPress sử dụng công cụ tự động hóa n8n.

## Tài Liệu Hướng Dẫn

- [Hướng dẫn cài đặt n8n](docs/n8n-installation.md)
- [Hướng dẫn import template](docs/import-template.md)
- [Sơ đồ workflow](docs/workflow-diagram.md)
- [**Sơ đồ workflow cải tiến với hình ảnh đẹp**](docs/workflow-diagram-improved.md) ← **MỚI!**
- [**Hướng dẫn chi tiết cấu hình từng node**](docs/huong-dan-chi-tiet-n8n.md)

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

3. **Import Workflow**
   - Tải template từ [templates/wordpress-auto-post-template.json](templates/wordpress-auto-post-template.json)
   - Trong n8n, nhấp vào "Import from File"
   - Chọn file đã tải xuống
   - Xem hướng dẫn chi tiết tại [docs/import-template.md](docs/import-template.md)

4. **Cấu hình Workflow**
   - Cập nhật thông tin RSS Feed (URL của kênh YouTube)
   - Cập nhật thông tin WordPress (URL, username, password)
   - Cấu hình các tùy chọn khác theo nhu cầu
   - Xem hướng dẫn chi tiết tại [docs/huong-dan-chi-tiet-n8n.md](docs/huong-dan-chi-tiet-n8n.md)

## Chi Tiết Workflow

Template workflow bao gồm các node sau:

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

4. **IF Node**
   - Kiểm tra xem bài đăng đã được tạo thành công chưa

5. **Email Node**
   - Gửi thông báo khi bài đăng được tạo thành công

Xem sơ đồ workflow chi tiết và trực quan tại [docs/workflow-diagram-improved.md](docs/workflow-diagram-improved.md)

## Tùy Chỉnh Workflow

Bạn có thể tùy chỉnh workflow này bằng cách:
- Thay đổi trigger (sử dụng Schedule thay vì RSS)
- Thêm các node xử lý nội dung (như AI Text Generation)
- Thêm các node thông báo (Telegram, Slack)
- Thêm điều kiện đăng bài (IF node)

## Các File Trong Repository

- **[templates/wordpress-auto-post-template.json](templates/wordpress-auto-post-template.json)**: Template workflow n8n để import
- **[docs/n8n-installation.md](docs/n8n-installation.md)**: Hướng dẫn cài đặt n8n
- **[docs/import-template.md](docs/import-template.md)**: Hướng dẫn import template
- **[docs/workflow-diagram.md](docs/workflow-diagram.md)**: Sơ đồ workflow cơ bản
- **[docs/workflow-diagram-improved.md](docs/workflow-diagram-improved.md)**: Sơ đồ workflow cải tiến với hình ảnh đẹp
- **[docs/huong-dan-chi-tiet-n8n.md](docs/huong-dan-chi-tiet-n8n.md)**: Hướng dẫn chi tiết cấu hình từng node

## Tài Liệu Tham Khảo

- [Tài liệu chính thức của n8n](https://docs.n8n.io/)
- [WordPress REST API Handbook](https://developer.wordpress.org/rest-api/)
- [n8n WordPress Node](https://docs.n8n.io/integrations/builtin/app-nodes/n8n-nodes-base.wordpress/)

## Đóng Góp

Nếu bạn có cải tiến hoặc workflow mới, hãy tạo pull request hoặc issue!