# Hướng Dẫn Chi Tiết Cấu Hình Workflow n8n Đăng Bài Tự Động Lên WordPress

## Mục lục
1. [Giới thiệu](#giới-thiệu)
2. [Cài đặt n8n](#cài-đặt-n8n)
3. [Import template workflow](#import-template-workflow)
4. [Cấu hình từng node](#cấu-hình-từng-node)
   - [Node RSS Feed](#1-node-rss-feed)
   - [Node Prepare WordPress Post](#2-node-prepare-wordpress-post)
   - [Node WordPress](#3-node-wordpress)
   - [Node IF (Post Created)](#4-node-if-post-created)
   - [Node Send Email](#5-node-send-email)
5. [Kích hoạt và kiểm tra workflow](#kích-hoạt-và-kiểm-tra-workflow)
6. [Xử lý sự cố thường gặp](#xử-lý-sự-cố-thường-gặp)
7. [Tùy chỉnh nâng cao](#tùy-chỉnh-nâng-cao)

## Giới thiệu

Workflow này giúp bạn tự động đăng bài từ kênh YouTube (hoặc bất kỳ nguồn RSS nào) lên website WordPress của bạn. Quy trình hoạt động như sau:

1. Lấy nội dung mới từ feed RSS
2. Xử lý và định dạng nội dung
3. Tạo bài đăng mới trên WordPress
4. Gửi email thông báo khi đăng bài thành công

<!-- Hình 1: Sơ đồ tổng quan workflow -->

## Cài đặt n8n

Trước khi bắt đầu, bạn cần cài đặt n8n trên máy chủ của mình:

### Cài đặt qua Docker (Khuyến nghị)

```bash
# Chạy n8n với Docker
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### Cài đặt qua npm

```bash
# Cài đặt n8n globally
npm install n8n -g

# Chạy n8n
n8n start
```

Sau khi cài đặt, truy cập n8n tại địa chỉ: http://localhost:5678

<!-- Hình 2: Giao diện n8n sau khi cài đặt -->

## Import template workflow

1. Tải template từ repository:
   - Truy cập: https://github.com/insightfreedom247/well-come-youtube/blob/main/templates/wordpress-auto-post-template.json
   - Nhấn nút "Raw" để xem nội dung JSON thuần túy
   - Nhấn Ctrl+S (hoặc Command+S trên Mac) để lưu file về máy

<!-- Hình 3: Tải template từ GitHub -->

2. Import vào n8n:
   - Trong n8n, nhấp vào "Workflows" trong menu bên trái
   - Nhấp vào nút "Import from File"
   - Chọn file JSON đã tải về
   - Nhấp "Import"

<!-- Hình 4: Import workflow vào n8n -->

3. Sau khi import, bạn sẽ thấy workflow với 5 node:
   - RSS Feed
   - Prepare WordPress Post
   - WordPress
   - IF (Post Created)
   - Send Email

<!-- Hình 5: Workflow sau khi import -->

## Cấu hình từng node

### 1. Node RSS Feed

**Mục đích**: Lấy nội dung từ nguồn RSS (có thể là kênh YouTube hoặc bất kỳ nguồn RSS nào)

**Cách cấu hình**:
1. Nhấp vào node "RSS Feed"
2. Cấu hình các thông số:
   - **URL**: Nhập URL feed RSS của kênh YouTube
     - Đối với YouTube, sử dụng định dạng: `https://www.youtube.com/feeds/videos.xml?channel_id=YOUR_CHANNEL_ID`
     - Để lấy Channel ID: Vào kênh YouTube > Nhấp chuột phải > Xem nguồn trang > Tìm "channelId"
   - **Options**:
     - **returnAll**: Chọn `false` để giới hạn số lượng bài
     - **limit**: Nhập số lượng bài muốn lấy (ví dụ: 5)
3. Chuyển sang tab "Trigger"
   - Điều chỉnh "Polling Interval" (khoảng thời gian kiểm tra, mặc định là 5 phút)
4. Nhấp "Execute Node" để kiểm tra kết quả

<!-- Hình 6: Cấu hình node RSS Feed -->
<!-- Hình 7: Kết quả sau khi thực thi node RSS Feed -->

### 2. Node Prepare WordPress Post

**Mục đích**: Xử lý dữ liệu từ RSS Feed và định dạng để đăng lên WordPress

**Cách cấu hình**:
1. Nhấp vào node "Prepare WordPress Post"
2. Kiểm tra code và điều chỉnh các thông số:
   ```javascript
   // Xử lý dữ liệu từ RSS Feed
   for (const item of items) {
     // Lấy tiêu đề và nội dung từ item
     const title = item.title;
     const content = item.content || item.summary || item.description;
     const link = item.link;
     
     // Tạo nội dung WordPress
     item.wordpress = {
       title,
       content: `${content}<p>Nguồn: <a href="${link}">${link}</a></p>`,
       status: "draft", // Có thể đổi thành "publish" nếu muốn đăng ngay
       categories: [1], // ID của category, thay đổi theo WordPress site của bạn
       featured_media: 0, // ID của ảnh đại diện nếu có
       excerpt: item.summary || ""
     };
   }
   
   return items;
   ```
3. Điều chỉnh các thông số:
   - **status**: 
     - "draft" - Lưu bài viết dưới dạng bản nháp (để kiểm tra trước khi đăng)
     - "publish" - Đăng bài ngay lập tức
   - **categories**: Thay đổi số 1 thành ID danh mục thực tế của bạn
   - **featured_media**: Thay đổi số 0 thành ID của ảnh đại diện (nếu có)
4. Nhấp "Execute Node" để kiểm tra kết quả

<!-- Hình 8: Cấu hình node Prepare WordPress Post -->
<!-- Hình 9: Kết quả sau khi thực thi node Prepare WordPress Post -->

#### Cách tìm ID danh mục trong WordPress:
1. Đăng nhập vào WordPress admin
2. Vào Bài viết > Chuyên mục
3. Di chuột vào danh mục bạn muốn sử dụng
4. Xem URL ở góc dưới trình duyệt, số trong "tag_ID=X" chính là ID danh mục

<!-- Hình 10: Cách tìm ID danh mục trong WordPress -->

### 3. Node WordPress

**Mục đích**: Kết nối với WordPress và tạo bài đăng mới

**Cách cấu hình**:
1. Nhấp vào node "WordPress"
2. Cấu hình thông tin kết nối:
   - **Authentication**: Chọn "Application Password" (khuyến nghị) hoặc "Basic Auth"
   - **URL**: Nhập URL của website WordPress của bạn (ví dụ: https://example.com)
   - **Username**: Nhập tên người dùng WordPress của bạn
   - **Password**: Nhập mật khẩu ứng dụng (nếu dùng Application Password) hoặc mật khẩu WordPress (nếu dùng Basic Auth)
3. Cấu hình thông tin bài đăng:
   - **Resource**: Chọn "Post"
   - **Operation**: Chọn "Create"
   - **Title**: Để nguyên `={{ $json.wordpress.title }}`
   - **Content**: Để nguyên `={{ $json.wordpress.content }}`
   - **Status**: Để nguyên `={{ $json.wordpress.status }}`
   - **Categories**: Để nguyên `={{ $json.wordpress.categories }}`
   - **Additional Fields**:
     - **featured_media**: Để nguyên `={{ $json.wordpress.featured_media }}`
     - **excerpt**: Để nguyên `={{ $json.wordpress.excerpt }}`
4. Nhấp "Execute Node" để kiểm tra kết quả (lưu ý: bước này sẽ tạo bài đăng thật trên WordPress)

<!-- Hình 11: Cấu hình node WordPress -->
<!-- Hình 12: Cấu hình thông tin kết nối WordPress -->
<!-- Hình 13: Cấu hình thông tin bài đăng -->

#### Cách tạo Application Password trong WordPress:
1. Đăng nhập vào WordPress admin
2. Vào Người dùng > Hồ sơ của bạn
3. Kéo xuống phần "Application Passwords"
4. Nhập tên (ví dụ: "n8n Automation")
5. Nhấp "Thêm mới"
6. Sao chép mật khẩu được tạo ra (chỉ hiển thị một lần)

<!-- Hình 14: Cách tạo Application Password trong WordPress -->

### 4. Node IF (Post Created)

**Mục đích**: Kiểm tra xem bài đăng đã được tạo thành công chưa

**Cách cấu hình**:
1. Nhấp vào node "IF (Post Created)"
2. Kiểm tra điều kiện:
   - **Value 1**: `{{ $json.success }}`
   - **Value 2**: `true`
3. Nhấp "Execute Node" để kiểm tra kết quả

<!-- Hình 15: Cấu hình node IF (Post Created) -->

### 5. Node Send Email

**Mục đích**: Gửi email thông báo khi bài đăng được tạo thành công

**Cách cấu hình**:
1. Nhấp vào node "Send Email"
2. Cấu hình thông tin email:
   - **To**: Nhập địa chỉ email của bạn
   - **Subject**: Để nguyên `New Post Created: {{ $json.wordpress.title }}`
   - **Text**: Để nguyên nội dung email
3. Cấu hình SMTP:
   - Nhấp vào "Credentials" > "Create New"
   - Chọn dịch vụ email (Gmail, SMTP, v.v.)
   - Nhập thông tin SMTP:
     - **Host**: smtp.gmail.com (nếu dùng Gmail)
     - **Port**: 465 (SSL) hoặc 587 (TLS)
     - **Secure**: Chọn SSL hoặc TLS
     - **User**: Địa chỉ email của bạn
     - **Password**: Mật khẩu email hoặc mật khẩu ứng dụng (với Gmail)
4. Nhấp "Execute Node" để kiểm tra kết quả

<!-- Hình 16: Cấu hình node Send Email -->
<!-- Hình 17: Cấu hình SMTP cho email -->

## Kích hoạt và kiểm tra workflow

1. Sau khi cấu hình xong tất cả các node, nhấp vào nút "Active" ở góc trên bên phải để kích hoạt workflow
2. Workflow sẽ chạy theo lịch trình được cấu hình trong node RSS Feed (mặc định là 5 phút một lần)
3. Kiểm tra WordPress để xem bài đăng mới
4. Kiểm tra email để xem thông báo

<!-- Hình 18: Kích hoạt workflow -->
<!-- Hình 19: Bài đăng mới trên WordPress -->
<!-- Hình 20: Email thông báo -->

## Xử lý sự cố thường gặp

### 1. Lỗi kết nối WordPress

**Nguyên nhân có thể**:
- URL WordPress không chính xác
- Thông tin đăng nhập không đúng
- REST API bị tắt hoặc bị chặn

**Cách khắc phục**:
- Kiểm tra URL WordPress (phải bao gồm https:// hoặc http://)
- Kiểm tra lại username và password
- Kiểm tra xem REST API có hoạt động không bằng cách truy cập: `https://your-wordpress-site.com/wp-json/wp/v2/posts`

### 2. Lỗi RSS Feed

**Nguyên nhân có thể**:
- URL feed không chính xác
- Feed không tồn tại hoặc không hoạt động
- Feed không có nội dung mới

**Cách khắc phục**:
- Kiểm tra URL feed bằng cách mở trong trình duyệt
- Thử feed khác để xác nhận node hoạt động
- Kiểm tra xem feed có nội dung mới không

### 3. Lỗi gửi email

**Nguyên nhân có thể**:
- Cấu hình SMTP không chính xác
- Tài khoản email không cho phép ứng dụng kém an toàn (với Gmail)
- Firewall chặn kết nối SMTP

**Cách khắc phục**:
- Kiểm tra lại thông tin SMTP
- Với Gmail, bật "Cho phép ứng dụng kém an toàn" hoặc tạo mật khẩu ứng dụng
- Kiểm tra firewall và cấu hình mạng

## Tùy chỉnh nâng cao

### 1. Thay đổi nguồn dữ liệu

Bạn có thể thay thế RSS Feed bằng các nguồn khác:
- **YouTube API**: Để lấy thông tin chi tiết hơn về video
- **HTTP Request**: Để lấy dữ liệu từ API khác
- **Schedule Trigger**: Để chạy workflow theo lịch cụ thể

### 2. Xử lý nội dung nâng cao

Bạn có thể thêm các node xử lý nội dung:
- **Text Manipulation**: Để định dạng nội dung
- **HTML Extract**: Để trích xuất thông tin từ HTML
- **OpenAI**: Để tóm tắt hoặc viết lại nội dung

### 3. Thêm điều kiện lọc

Bạn có thể thêm các điều kiện để lọc nội dung:
- **IF Node**: Để kiểm tra nội dung trước khi đăng
- **Switch Node**: Để xử lý nhiều trường hợp khác nhau
- **Filter Node**: Để lọc dữ liệu theo tiêu chí

### 4. Thêm thông báo khác

Ngoài email, bạn có thể thêm các thông báo khác:
- **Telegram**: Gửi thông báo qua Telegram
- **Slack**: Gửi thông báo qua Slack
- **Discord**: Gửi thông báo qua Discord

<!-- Hình 21: Ví dụ về workflow nâng cao -->

---

## Kết luận

Với workflow này, bạn có thể tự động hóa việc đăng bài từ YouTube lên WordPress, tiết kiệm thời gian và công sức. Hãy thử nghiệm và tùy chỉnh workflow theo nhu cầu của bạn!

Nếu bạn cần hỗ trợ thêm, hãy tạo issue trên [GitHub repository](https://github.com/insightfreedom247/well-come-youtube) hoặc tham khảo [tài liệu chính thức của n8n](https://docs.n8n.io/).

---

**Lưu ý**: Các hình ảnh minh họa nên được đặt tại vị trí các comment `<!-- Hình X: Mô tả -->` trong tài liệu này.