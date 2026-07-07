# SoCom — NotebookLM System Design Prompt

Tài liệu này được thiết kế theo dạng văn bản có cấu trúc logic (Structured Text). Bạn có thể sao chép toàn bộ phần nội dung bên dưới và dán vào **NotebookLM** (hoặc ChatGPT, Claude, Eraser.io, Mermaid Live) để yêu cầu AI tự động generate ra bản vẽ kiến trúc, hoặc để NotebookLM "học" và tóm tắt luồng dữ liệu của hệ thống.

---

## [COPY ĐOẠN DƯỚI ĐÂY ĐỂ ĐƯA VÀO AI]

**Ngữ cảnh:**
Bạn là một System Architect tài năng. Dựa vào mô tả chi tiết của nền tảng SoCom (Hệ sinh thái Live Commerce Management) dưới đây, hãy phân tích và tạo ra một sơ đồ kiến trúc hệ thống (System Architecture Diagram). Nếu công cụ của bạn hỗ trợ tạo code (như Mermaid.js), hãy xuất ra đoạn code Mermaid (graph TD). Nếu bạn tạo text/tóm tắt, hãy mô tả thật trực quan các luồng giao tiếp giữa các node.

### 1. Các nhóm (Clusters) và Thành phần (Nodes)

**Cluster 1: Client Layer (Tầng người dùng)**
*   Node: "Vue.js SPA" (Chứa các giao diện: Mod View, Director Flying Desk, Host Prompter)

**Cluster 2: Edge & Routing (Tầng mạng ngoại vi AWS)**
*   Node: "CloudFront & WAF" (CDN & Bảo mật Web)
*   Node: "AWS ALB / NLB" (Load Balancers)

**Cluster 3: Identity & Security (Tầng định danh)**
*   Node: "Shared Foundation Service" (Service nội bộ viết bằng Golang, quản lý Multi-tenant & Phân quyền)
*   Node: "AWS IAM / Cognito" (Core Identity engine của AWS)

**Cluster 4: Core Control Plane (Tầng dịch vụ lõi - Golang)**
*   Node: "API Gateway & WebSockets" (Cổng giao tiếp chính, duy trì kết nối real-time)
*   Node: "Scheduling Service" (Quản lý lịch, giải quyết xung đột tài nguyên)
*   Node: "Run of Show (ROS) Service" (Quản lý kịch bản livestream)
*   Node: "Omni-channel Engine" (Xử lý webhooks và scraping dữ liệu đa sàn)
*   Node: "Billing & Analytics Service" (Xử lý tính lương, báo cáo)

**Cluster 5: Data Plane & AI Workers (Tầng AI & Xử lý nền - Python)**
*   Node: "Generative AI Worker" (Tạo ảnh, video từ API Kling, Veo)
*   Node: "NLP & Reconcile Worker" (Chạy mô hình Speech-to-Text, kiểm duyệt từ cấm)
*   Node: "Media Processing Worker" (Chạy FFmpeg, cắt clip 9:16)
*   Node: "Social Publisher Worker" (Tự động đăng bài lên các MXH)

**Cluster 6: Async Messaging (Tầng trung gian bất đồng bộ)**
*   Node: "RabbitMQ (Amazon MQ)" (Message Broker quản lý Event và Task Queues)

**Cluster 7: Persistence Layer (Tầng lưu trữ dữ liệu)**
*   Node: "Amazon RDS (MySQL)" (Lưu Master Data: User, Billing, Booking)
*   Node: "Amazon OpenSearch" (Lưu Log chat, Vector search)
*   Node: "Amazon ElastiCache (Redis)" (Lưu Cache, WebSocket Pub/Sub, Distributed Lock)
*   Node: "Amazon S3" (Lưu trữ file Media, Record Livestream)

---

### 2. Mối quan hệ và luồng giao tiếp (Edges/Relationships)

1. "Vue.js SPA" gửi request (HTTPS/WSS) tới "CloudFront & WAF".
2. "CloudFront & WAF" trỏ lưu lượng tới "AWS ALB / NLB".
3. "AWS ALB / NLB" chia tải vào "Shared Foundation Service" (để lấy JWT Auth) HOẶC "API Gateway & WebSockets" (để thực hiện nghiệp vụ).
4. "Shared Foundation Service" giao tiếp 2 chiều với "AWS IAM / Cognito" để verify danh tính.
5. "API Gateway & WebSockets" xác thực JWT và gọi tới 4 node của "Core Control Plane" thông qua gRPC/HTTP nội bộ.
6. Các node trong "Core Control Plane" thực hiện thao tác Đọc/Ghi dữ liệu tới cụm "Persistence Layer" (MySQL, OpenSearch, Redis).
7. "Omni-channel Engine" và "API Gateway" đẩy sự kiện (Publish Events) vào "RabbitMQ (Amazon MQ)".
8. Các node trong "Data Plane & AI Workers" liên tục kéo (Consume Tasks) từ "RabbitMQ (Amazon MQ)".
9. "Data Plane & AI Workers" lưu kết quả xử lý media vào "Amazon S3".
10. "API Gateway & WebSockets" đồng bộ trạng thái kết nối thông qua "Amazon ElastiCache (Redis)".

---

### 3. Yêu cầu vẽ (Rendering Instructions)
*   **Hướng vẽ:** Top-Down (Từ trên xuống dưới).
*   **Gom nhóm (Grouping):** Đặt cụm Cluster 3, 4, 5, 6, 7 vào chung một box lớn, đặt tên là "AWS Cloud - Private VPC".
*   **Màu sắc (nếu có thể):** Sử dụng các gam màu phân tách rõ ràng cho Golang Services, Python AI Workers và Database. Mũi tên gọi đồng bộ (Sync) là nét liền, mũi tên gọi bất đồng bộ qua RabbitMQ (Async) là nét đứt.
