# SoCom — System Design & Architecture

**Last updated:** July 2026
**Domain:** socom.io (placeholder)

---

## Table of Contents

1. [High-Level Architecture](#1-high-level-architecture)
2. [Tech Stack & Infrastructure](#2-tech-stack--infrastructure)
3. [Service Modules Breakdown](#3-service-modules-breakdown)
4. [Detailed Data Flow & Integration Points](#4-detailed-data-flow--integration-points)

---

## 1. High-Level Architecture

Dưới đây là sơ đồ kiến trúc tổng thể của SoCom, được thiết kế theo hướng Event-Driven Microservices, đảm bảo tính high-availability và real-time cho hệ thống Live Commerce.

```text
                               +---------------------------------------+
                               |        Client Applications            |
                               | (Vue.js - Mod View, Flying Desk, etc) |
                               +-------------------+-------------------+
                                                   |
                                       +-----------+-----------+
                                       |  AWS CloudFront + WAF |
                                       +-----------+-----------+
                                                   |
                                         +---------+---------+
                                         |    AWS ALB / NLB  |
                                         +---------+---------+
                                                   |
+----------------------+         +-----------------v-----------------+
| Shared Foundation    | <-----> |     API Gateway (Golang/Gin)      |
| Service (Auth Node)  |         | (Rate Limiting, Routing, Socket)  |
| - Wraps AWS IAM      |         +-----------------+-----------------+
| - Tenant/RBAC Mgmt   |                           |
+----------------------+                           | (gRPC / HTTP)
                                                   |
             +-------------------------------------+---------------------------------------+
             |                                                                             |
+------------v-----------+ +------------v-----------+ +------------v-----------+ +---------v---------+
| Resource & Scheduling  | |   Run of Show (ROS)    | |  Omni-channel Engine   | | Analytics & Billing |
| Service (Golang)       | |   Service (Golang)     | |  Service (Golang)      | | Service (Golang)    |
+------------+-----------+ +------------+-----------+ +------------+-----------+ +---------+---------+
             |                          |                          |                       |
             +--------------------------+--------------------------+-----------------------+
                                                   |
                                       +-----------v-----------+
                                       |  Message Broker       |
                                       | (Amazon MQ / Redis)   |
                                       +-----------+-----------+
                                                   |
             +-------------------------------------+---------------------------------------+
             |                                                                             |
+------------v-----------+ +------------v-----------+ +------------v-----------+ +---------v---------+
| Generative AI Wrapper  | | NLP & Reconcile Worker | | Media Processing Node  | | Social Publisher    |
| Worker (Python)        | | (Python)               | | (Python / FFmpeg)      | | Worker (Python)     |
+------------+-----------+ +------------+-----------+ +------------+-----------+ +---------+---------+
             |                          |                          |                       |
             +--------------------------+--------------------------+-----------------------+
                                                   |
                                       +-----------v-----------+
                                       |    Persistence Layer  |
                                       +-----------------------+
                                       | - Amazon RDS (MySQL)  |
                                       | - Amazon OpenSearch   |
                                       | - Amazon ElastiCache  |
                                       +-----------------------+
```

---

## 2. Tech Stack & Infrastructure

Toàn bộ hệ thống được deploy trên hạ tầng **AWS**, chia tách rạch ròi giữa Control Plane (điều hướng, logic) và Data Plane (xử lý dữ liệu, AI).

*   **Frontend (FE):** **Vue.js 3** kết hợp Vite và Pinia. Vue.js cực kỳ phù hợp cho các Dashboard đòi hỏi tính real-time cao (Mod View, Flying Desk) nhờ hệ thống reactivity nhẹ và linh hoạt, giúp render hàng ngàn dòng chat mà không bị giật lag (re-render tối ưu).
*   **Identity & Security:** Sử dụng **AWS IAM** làm core engine, nhưng không expose trực tiếp cho Client mà được bọc (wrapper) qua **Shared Foundation Service**.
*   **Backend Core (Control Plane):** **Golang (Gin / Fiber)**. Đảm nhận xử lý I/O cường độ cao, API Gateway, và giữ kết nối WebSockets (Stateful).
*   **Backend AI/Data (Data Plane):** **Python (FastAPI / Celery)**. Đảm nhận giao tiếp với các mô hình AI ngoại lai (Veo, Kling), chạy model Local NLP, và xử lý FFmpeg.
*   **Database & Cache:**
    *   **Amazon RDS (MySQL/PostgreSQL):** Lưu trữ Master Data (Billing, Booking, Profiles) đòi hỏi tính toàn vẹn (ACID).
    *   **Amazon OpenSearch:** Tìm kiếm vector (để match Host với Brand) và lưu trữ log chat thống nhất tốc độ cao.
    *   **Amazon ElastiCache (Redis):** Distributed Caching, khóa chống xung đột (Distributed Locks) khi booking, và Redis Pub/Sub cho WebSockets.
*   **Message Queue:** **Amazon MQ (RabbitMQ)** để chạy hàng đợi xử lý bất đồng bộ (Video Gen, Auto-post).

---

## 3. Service Modules Breakdown

### 3.1. Shared Foundation Service (Identity & Tenant Management)
Đây là service "gác cổng" độc lập, định tuyến toàn bộ quyền truy cập trong hệ thống SaaS đa khách hàng (Multi-tenant).
*   **Nhiệm vụ:** Quản lý User, Workspace, và Phân quyền (RBAC).
*   **Kiến trúc:** Act như một lớp Abstraction (Wrapper) đè lên **AWS IAM** (hoặc AWS Cognito). Thay vì tạo trực tiếp IAM User cho mỗi nhân viên của MCN (gây quá tải và khó quản lý logic nghiệp vụ), hệ thống dùng IAM Role trung tâm, và Shared Foundation Service sẽ map User ID nội bộ thành các quyền (Policies) tương ứng.
*   **Cơ chế:** Khi user login thành công, service trả về JWT Token chứa `Workspace_ID` và `Role`. Các API khác chỉ cần verify chữ ký của JWT này (Stateless) mà không cần gọi lại database.

### 3.2. API Gateway & Stateful WebSockets
*   **Nhiệm vụ:** Tiếp nhận mọi request từ Vue.js FE.
*   **WebSockets:** Sử dụng các thư viện như `Centrifugo` (viết bằng Go) để quản lý hàng triệu kết nối WebSocket đồng thời. Đảm bảo luồng chat từ Mod View và máy nhắc chữ (Prompter) realtime với độ trễ < 50ms. API Gateway sẽ đứng sau AWS NLB (Network Load Balancer) thay vì ALB để tối ưu kết nối TCP kéo dài.

### 3.3. Core Microservices (Golang)
*   **Resource & Scheduling Service:** Động cơ giải quyết xung đột (Conflict Resolution). Dùng thuật toán Distributed Lock qua Redis để đảm bảo 2 brand không thể book trùng 1 phòng studio ở cùng 1 thời điểm.
*   **Run of Show (ROS) Service:** Quản lý kịch bản. Chịu trách nhiệm đồng bộ vị trí kịch bản (scroll position) từ màn hình của Director sang màn hình Prompter của Host.
*   **Omni-channel Engine:** Giao tiếp với Webhooks của TikTok/Shopee, và điều khiển các Internal Scraping Workers để hứng log chat, mắt xem (CCU) và đẩy vào OpenSearch.
*   **Analytics & Billing Service:** Tính toán lương, hoa hồng affiliate và phạt vi phạm tự động dựa trên Data sinh ra từ luồng Live.

### 3.4. AI & Media Workers (Python)
*   **Generative AI Wrapper:** Nhận Job (như tạo teaser) từ Message Broker, gọi API sang ElevenLabs/Veo 3, chờ kết quả, download file về AWS S3, và bắn webhook báo thành công.
*   **NLP & Reconcile Worker:** Bóc băng âm thanh (STT) theo thời gian thực (Realtime), đối chiếu với kịch bản gốc và danh sách từ cấm (Banned words).
*   **Media Processing Node:** Tích hợp FFmpeg. Sau khi phiên live kết thúc, tự động tải record về, cắt clip dọc (9:16), tracking khuôn mặt, render subtitle tĩnh/động, và lưu lại vào S3.
*   **Social Publisher Worker:** Nhận lịch đăng bài từ Database, dùng thư viện auto-post đẩy video lên TikTok, YT Shorts theo đúng giờ đã định.

---

## 4. Detailed Data Flow & Integration Points

### A. Auth Flow (Shared Foundation Service)
1. Client (Vue.js) gửi request login (Username/Password hoặc SSO).
2. Request đi qua CloudFront -> ALB -> vào **Shared Foundation Service**.
3. Foundation Service map thông tin này với policy trong **AWS IAM** và DB phân quyền.
4. Foundation Service generate một JWT (đã ký) và trả về cho Client.
5. Từ đó, Client cầm JWT gọi vào các Core Services thông qua API Gateway.

### B. Real-time Chat Flow (Omni-channel -> Mod View)
1. Phiên live đang diễn ra, người dùng comment trên TikTok.
2. **Omni-channel Engine** bắt được event thông qua Webhook hoặc Headless Scraper.
3. Engine đẩy raw message vào hàng đợi **RabbitMQ** (Topic: `chat.incoming`).
4. **API Gateway (WebSocket Node)** consume message này. Dựa vào Session ID, Gateway tìm ra connection của Mod đang trực phòng đó.
5. Gateway bắn trực tiếp message xuống Vue.js Mod View qua WebSocket. Đồng thời, một Worker khác copy message đó lưu tĩnh vào **OpenSearch**.

### C. AI Moderation Flow (Cảnh báo từ cấm)
1. Audio stream từ phòng Studio được nén (chunks 10s) đẩy liên tục lên endpoint của hệ thống.
2. **NLP Worker** nhận audio chunk, chạy Local Model STT (ví dụ Whisper) để ra Text.
3. Worker quét Text qua Dictionary (Tập từ cấm của Brand/Sàn).
4. Nếu phát hiện vi phạm (VD: Host nói "Shopee" khi đang live TikTok):
   - Worker push event `violation_detected` vào RabbitMQ.
   - API Gateway nhận event, lập tức broadcast cảnh báo đỏ xuống màn hình Vue.js của Host (Prompter) và Mod.
   - Toàn bộ quá trình từ lúc Host nói đến lúc màn hình nháy đỏ mất < 1.5 giây.
