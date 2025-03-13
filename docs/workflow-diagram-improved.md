# Sơ Đồ Workflow n8n Cải Tiến

## Tổng Quan Workflow

```mermaid
graph LR
    A[YouTube RSS Feed] -->|Lấy video mới| B[Xử lý Nội Dung]
    B -->|Định dạng bài viết| C[WordPress API]
    C -->|Tạo bài đăng| D{Kiểm tra thành công}
    D -->|Thành công| E[Gửi Thông Báo]
    D -->|Thất bại| F[Ghi Log Lỗi]
    
    style A fill:#ff0000,stroke:#333,stroke-width:2px,color:white
    style B fill:#ffcc00,stroke:#333,stroke-width:2px
    style C fill:#00cc66,stroke:#333,stroke-width:2px,color:white
    style D fill:#0099ff,stroke:#333,stroke-width:2px,color:white
    style E fill:#9900cc,stroke:#333,stroke-width:2px,color:white
    style F fill:#ff6600,stroke:#333,stroke-width:2px,color:white
```

## Chi Tiết Các Node

### 1. YouTube RSS Feed Node

```mermaid
graph TD
    A[YouTube RSS Feed] --> B[Cấu hình]
    B --> C[URL Feed]
    B --> D[Polling Interval]
    B --> E[Limit]
    
    style A fill:#ff0000,stroke:#333,stroke-width:2px,color:white
    style B fill:#f9f9f9,stroke:#333,stroke-width:1px
    style C fill:#f0f0f0,stroke:#333,stroke-width:1px
    style D fill:#f0f0f0,stroke:#333,stroke-width:1px
    style E fill:#f0f0f0,stroke:#333,stroke-width:1px
```

**Cấu hình chi tiết:**
- **URL Feed**: `https://www.youtube.com/feeds/videos.xml?channel_id=YOUR_CHANNEL_ID`
- **Polling Interval**: Tần suất kiểm tra feed (mặc định: 5 phút)
- **Limit**: Số lượng video mới nhất muốn lấy (ví dụ: 5)

### 2. Xử Lý Nội Dung Node

```mermaid
graph TD
    A[Function Node] --> B[Input]
    B --> C[RSS Items]
    A --> D[Process]
    D --> E[Extract Title]
    D --> F[Extract Content]
    D --> G[Extract Link]
    A --> H[Output]
    H --> I[WordPress Format]
    
    style A fill:#ffcc00,stroke:#333,stroke-width:2px
    style B fill:#f9f9f9,stroke:#333,stroke-width:1px
    style C fill:#f0f0f0,stroke:#333,stroke-width:1px
    style D fill:#f9f9f9,stroke:#333,stroke-width:1px
    style E fill:#f0f0f0,stroke:#333,stroke-width:1px
    style F fill:#f0f0f0,stroke:#333,stroke-width:1px
    style G fill:#f0f0f0,stroke:#333,stroke-width:1px
    style H fill:#f9f9f9,stroke:#333,stroke-width:1px
    style I fill:#f0f0f0,stroke:#333,stroke-width:1px
```

**Code xử lý:**
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
    status: "draft",
    categories: [1],
    featured_media: 0,
    excerpt: item.summary || ""
  };
}

return items;
```

### 3. WordPress API Node

```mermaid
graph TD
    A[WordPress Node] --> B[Authentication]
    B --> C[Application Password]
    B --> D[Basic Auth]
    A --> E[Connection]
    E --> F[URL]
    E --> G[Username]
    E --> H[Password]
    A --> I[Post Configuration]
    I --> J[Title]
    I --> K[Content]
    I --> L[Status]
    I --> M[Categories]
    
    style A fill:#00cc66,stroke:#333,stroke-width:2px,color:white
    style B fill:#f9f9f9,stroke:#333,stroke-width:1px
    style C fill:#f0f0f0,stroke:#333,stroke-width:1px
    style D fill:#f0f0f0,stroke:#333,stroke-width:1px
    style E fill:#f9f9f9,stroke:#333,stroke-width:1px
    style F fill:#f0f0f0,stroke:#333,stroke-width:1px
    style G fill:#f0f0f0,stroke:#333,stroke-width:1px
    style H fill:#f0f0f0,stroke:#333,stroke-width:1px
    style I fill:#f9f9f9,stroke:#333,stroke-width:1px
    style J fill:#f0f0f0,stroke:#333,stroke-width:1px
    style K fill:#f0f0f0,stroke:#333,stroke-width:1px
    style L fill:#f0f0f0,stroke:#333,stroke-width:1px
    style M fill:#f0f0f0,stroke:#333,stroke-width:1px
```

**Cấu hình chi tiết:**
- **Authentication**: Application Password (khuyến nghị)
- **URL**: URL của website WordPress (ví dụ: https://example.com)
- **Username**: Tên người dùng WordPress
- **Password**: Mật khẩu ứng dụng hoặc mật khẩu thông thường
- **Resource**: Post
- **Operation**: Create

### 4. Kiểm Tra Thành Công Node

```mermaid
graph TD
    A[IF Node] --> B[Condition]
    B --> C["success === true"]
    A --> D[True]
    D --> E[Send Email]
    A --> F[False]
    F --> G[Log Error]
    
    style A fill:#0099ff,stroke:#333,stroke-width:2px,color:white
    style B fill:#f9f9f9,stroke:#333,stroke-width:1px
    style C fill:#f0f0f0,stroke:#333,stroke-width:1px
    style D fill:#f9f9f9,stroke:#333,stroke-width:1px
    style E fill:#f0f0f0,stroke:#333,stroke-width:1px
    style F fill:#f9f9f9,stroke:#333,stroke-width:1px
    style G fill:#f0f0f0,stroke:#333,stroke-width:1px
```

**Điều kiện:**
- **Value 1**: `{{ $json.success }}`
- **Value 2**: `true`

### 5. Gửi Thông Báo Node

```mermaid
graph TD
    A[Email Node] --> B[Configuration]
    B --> C[To]
    B --> D[Subject]
    B --> E[Body]
    A --> F[SMTP Setup]
    F --> G[Host]
    F --> H[Port]
    F --> I[User]
    F --> J[Password]
    
    style A fill:#9900cc,stroke:#333,stroke-width:2px,color:white
    style B fill:#f9f9f9,stroke:#333,stroke-width:1px
    style C fill:#f0f0f0,stroke:#333,stroke-width:1px
    style D fill:#f0f0f0,stroke:#333,stroke-width:1px
    style E fill:#f0f0f0,stroke:#333,stroke-width:1px
    style F fill:#f9f9f9,stroke:#333,stroke-width:1px
    style G fill:#f0f0f0,stroke:#333,stroke-width:1px
    style H fill:#f0f0f0,stroke:#333,stroke-width:1px
    style I fill:#f0f0f0,stroke:#333,stroke-width:1px
    style J fill:#f0f0f0,stroke:#333,stroke-width:1px
```

**Cấu hình chi tiết:**
- **To**: Địa chỉ email nhận thông báo
- **Subject**: `New Post Created: {{ $json.wordpress.title }}`
- **Body**: Nội dung thông báo về bài đăng mới

## Luồng Dữ Liệu Chi Tiết

```mermaid
sequenceDiagram
    participant Y as YouTube
    participant R as RSS Feed Node
    participant F as Function Node
    participant W as WordPress Node
    participant I as IF Node
    participant E as Email Node
    
    Y->>R: Cung cấp video mới
    R->>F: Chuyển dữ liệu RSS
    F->>F: Xử lý và định dạng dữ liệu
    F->>W: Gửi dữ liệu đã định dạng
    W->>W: Tạo bài đăng WordPress
    W->>I: Trả về kết quả
    I->>I: Kiểm tra thành công
    I->>E: Nếu thành công
    E->>E: Gửi email thông báo
```

## Cấu Trúc Dữ Liệu

### Input từ RSS Feed

```json
{
  "title": "Tiêu đề video YouTube",
  "link": "https://www.youtube.com/watch?v=VIDEO_ID",
  "pubDate": "2023-01-01T12:00:00Z",
  "author": "Tên kênh",
  "id": "yt:video:VIDEO_ID",
  "content": "Mô tả video",
  "contentSnippet": "Mô tả ngắn"
}
```

### Output từ Function Node

```json
{
  "title": "Tiêu đề video YouTube",
  "link": "https://www.youtube.com/watch?v=VIDEO_ID",
  "pubDate": "2023-01-01T12:00:00Z",
  "author": "Tên kênh",
  "id": "yt:video:VIDEO_ID",
  "content": "Mô tả video",
  "contentSnippet": "Mô tả ngắn",
  "wordpress": {
    "title": "Tiêu đề video YouTube",
    "content": "Mô tả video<p>Nguồn: <a href=\"https://www.youtube.com/watch?v=VIDEO_ID\">https://www.youtube.com/watch?v=VIDEO_ID</a></p>",
    "status": "draft",
    "categories": [1],
    "featured_media": 0,
    "excerpt": "Mô tả ngắn"
  }
}
```

### Output từ WordPress Node

```json
{
  "success": true,
  "id": 123,
  "permalink": "https://example.com/2023/01/01/tieu-de-video-youtube/"
}
```

## Tùy Chỉnh Workflow

### Thay Đổi Nguồn Dữ Liệu

```mermaid
graph LR
    A[YouTube API] -->|Thay thế| B[RSS Feed]
    C[Schedule Trigger] -->|Thay thế| B
    D[HTTP Request] -->|Thay thế| B
    
    style A fill:#ff9999,stroke:#333,stroke-width:1px
    style B fill:#ff0000,stroke:#333,stroke-width:2px,color:white
    style C fill:#ff9999,stroke:#333,stroke-width:1px
    style D fill:#ff9999,stroke:#333,stroke-width:1px
```

### Thêm Xử Lý Nội Dung

```mermaid
graph LR
    A[Function Node] -->|Thêm| B[OpenAI Node]
    B -->|Thêm| C[Text Manipulation]
    C --> D[WordPress Node]
    
    style A fill:#ffcc00,stroke:#333,stroke-width:2px
    style B fill:#ff99cc,stroke:#333,stroke-width:1px
    style C fill:#ff99cc,stroke:#333,stroke-width:1px
    style D fill:#00cc66,stroke:#333,stroke-width:2px,color:white
```

### Thêm Thông Báo

```mermaid
graph LR
    A[IF Node] -->|Thành công| B[Email Node]
    A -->|Thành công| C[Telegram Node]
    A -->|Thành công| D[Slack Node]
    
    style A fill:#0099ff,stroke:#333,stroke-width:2px,color:white
    style B fill:#9900cc,stroke:#333,stroke-width:2px,color:white
    style C fill:#cc99ff,stroke:#333,stroke-width:1px
    style D fill:#cc99ff,stroke:#333,stroke-width:1px
```

## Lưu Ý Quan Trọng

- Đảm bảo WordPress REST API được bật
- Sử dụng Application Password thay vì mật khẩu thông thường
- Ban đầu nên để trạng thái bài đăng là "draft" để kiểm tra
- Kiểm tra kết nối trước khi kích hoạt workflow
- Cập nhật ID danh mục WordPress chính xác