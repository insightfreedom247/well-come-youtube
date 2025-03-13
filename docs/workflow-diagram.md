# Sơ Đồ Workflow n8n Đăng Bài WordPress

Dưới đây là sơ đồ mô tả quy trình tự động đăng bài từ YouTube lên WordPress sử dụng n8n:

```
+----------------+     +-------------------+     +------------------+
|                |     |                   |     |                  |
|  YouTube API   +---->+  Xử lý nội dung   +---->+  WordPress API  |
|  hoặc RSS Feed |     |  (Format, Filter) |     |  (Create Post)   |
|                |     |                   |     |                  |
+----------------+     +-------------------+     +------------------+
        |                       |                        |
        |                       |                        |
        v                       v                        v
+----------------+     +-------------------+     +------------------+
|                |     |                   |     |                  |
|  Lấy metadata  |     |  Tạo thumbnail    |     |  Thông báo      |
|  (title, desc) |     |  hoặc ảnh đại diện|     |  (email, slack) |
|                |     |                   |     |                  |
+----------------+     +-------------------+     +------------------+
```

## Chi tiết các bước

1. **Nguồn dữ liệu**:
   - YouTube API: Lấy video mới từ kênh YouTube
   - RSS Feed: Theo dõi feed RSS của kênh YouTube
   - Webhook: Nhận thông báo khi có video mới

2. **Xử lý nội dung**:
   - Trích xuất tiêu đề, mô tả, link video
   - Định dạng nội dung (HTML, Markdown)
   - Lọc nội dung không mong muốn

3. **Tạo bài đăng WordPress**:
   - Kết nối với WordPress REST API
   - Tạo bài đăng mới với nội dung đã xử lý
   - Gán category, tag, và các metadata khác

4. **Hành động sau khi đăng**:
   - Gửi email thông báo
   - Đăng thông báo lên Slack/Discord
   - Cập nhật trạng thái trong database

## Cấu hình n8n

Để cấu hình workflow này trong n8n, bạn cần:

1. **Credentials**:
   - YouTube API key (nếu sử dụng YouTube API)
   - WordPress credentials (username/password hoặc application password)
   - SMTP credentials (nếu gửi email)

2. **Nodes**:
   - HTTP Request hoặc RSS Feed (nguồn dữ liệu)
   - Function (xử lý dữ liệu)
   - WordPress (đăng bài)
   - IF (điều kiện)
   - Email/Slack/Telegram (thông báo)

## Lưu ý

- Workflow có thể chạy theo lịch (mỗi giờ, mỗi ngày)
- Có thể thêm node lưu trữ để tránh đăng trùng lặp
- Nên bắt đầu với chế độ "draft" trước khi chuyển sang "publish"