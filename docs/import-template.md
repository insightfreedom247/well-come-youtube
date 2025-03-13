# Hướng Dẫn Import Template vào n8n

Dưới đây là hướng dẫn chi tiết cách import template workflow từ repository này vào n8n của bạn.

## Phương pháp 1: Import từ file JSON

1. **Tải file template**
   - Truy cập file template tại: [wordpress-auto-post-template.json](https://github.com/insightfreedom247/well-come-youtube/blob/main/templates/wordpress-auto-post-template.json)
   - Nhấn nút "Raw" để xem nội dung JSON thuần túy
   - Nhấn Ctrl+S (hoặc Command+S trên Mac) để lưu file về máy

2. **Import vào n8n**
   - Mở n8n của bạn (http://localhost:5678 hoặc URL của n8n.cloud)
   - Nhấp vào "Workflows" trong menu bên trái
   - Nhấp vào nút "Import from File" (hoặc "Import" > "Import from File")
   - Chọn file JSON đã tải về
   - Nhấp "Import"

## Phương pháp 2: Copy-Paste JSON

1. **Sao chép nội dung JSON**
   - Truy cập file template tại: [wordpress-auto-post-template.json](https://github.com/insightfreedom247/well-come-youtube/blob/main/templates/wordpress-auto-post-template.json)
   - Nhấn nút "Raw" để xem nội dung JSON thuần túy
   - Nhấn Ctrl+A (hoặc Command+A trên Mac) để chọn tất cả
   - Nhấn Ctrl+C (hoặc Command+C trên Mac) để sao chép

2. **Import vào n8n**
   - Mở n8n của bạn
   - Nhấp vào "Workflows" trong menu bên trái
   - Nhấp vào nút "Import" > "Import from JSON"
   - Dán nội dung JSON đã sao chép vào ô văn bản
   - Nhấp "Import"

## Phương pháp 3: Import từ URL

1. **Lấy URL của file raw**
   - URL raw của template: [https://raw.githubusercontent.com/insightfreedom247/well-come-youtube/main/templates/wordpress-auto-post-template.json](https://raw.githubusercontent.com/insightfreedom247/well-come-youtube/main/templates/wordpress-auto-post-template.json)

2. **Import vào n8n**
   - Mở n8n của bạn
   - Nhấp vào "Workflows" trong menu bên trái
   - Nhấp vào nút "Import" > "Import from URL"
   - Dán URL raw vào ô văn bản
   - Nhấp "Import"

## Sau khi import

Sau khi import thành công, bạn cần cấu hình một số thông tin:

1. **Cấu hình RSS Feed**
   - Nhấp vào node "RSS Feed"
   - Cập nhật URL feed thành URL của kênh YouTube hoặc nguồn RSS của bạn
   - Điều chỉnh các tùy chọn như số lượng bài viết, tần suất kiểm tra

2. **Cấu hình WordPress**
   - Nhấp vào node "WordPress"
   - Thêm thông tin xác thực WordPress của bạn:
     - URL của website WordPress
     - Username và password (hoặc application password)
   - Điều chỉnh các tùy chọn như trạng thái bài đăng, danh mục

3. **Cấu hình Email (nếu sử dụng)**
   - Nhấp vào node "Send Email"
   - Cập nhật địa chỉ email người nhận
   - Cấu hình SMTP nếu cần

4. **Kích hoạt Workflow**
   - Nhấp vào nút "Active" ở góc trên bên phải để kích hoạt workflow
   - Hoặc nhấp vào nút "Execute Workflow" để chạy thử

## Xử lý sự cố

Nếu bạn gặp lỗi khi import hoặc chạy workflow:

1. **Lỗi khi import**
   - Kiểm tra xem bạn đã sao chép đầy đủ nội dung JSON chưa
   - Thử làm mới trình duyệt và import lại

2. **Lỗi khi chạy workflow**
   - Kiểm tra các thông tin xác thực (WordPress, email)
   - Kiểm tra URL của RSS feed
   - Xem logs để biết thêm chi tiết về lỗi

## Tùy chỉnh nâng cao

Bạn có thể tùy chỉnh workflow này bằng cách:

1. **Thay đổi nguồn dữ liệu**
   - Thay thế RSS Feed bằng YouTube node để lấy video trực tiếp
   - Sử dụng HTTP Request để lấy dữ liệu từ API khác

2. **Thêm xử lý nội dung**
   - Thêm node OpenAI để tóm tắt hoặc viết lại nội dung
   - Thêm node Text Manipulation để định dạng nội dung

3. **Thêm điều kiện**
   - Thêm node IF để kiểm tra nội dung trước khi đăng
   - Lọc bài viết dựa trên từ khóa hoặc độ dài

4. **Thêm thông báo**
   - Thêm node Telegram, Slack hoặc Discord để gửi thông báo
   - Lưu log vào database để theo dõi

## Hỗ trợ

Nếu bạn cần hỗ trợ thêm:
- Tạo issue trong repository này
- Tham khảo [tài liệu chính thức của n8n](https://docs.n8n.io/)
- Tham gia [cộng đồng n8n](https://community.n8n.io/)