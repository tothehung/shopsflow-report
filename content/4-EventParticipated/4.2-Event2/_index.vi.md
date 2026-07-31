---
title: "Event 2"
date: 2026-07-11
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch "AWS Technical Deep Dive & Certification Roadmap (11/7)"

### Mục Đích Của Sự Kiện

- **Ứng dụng AI/Agent vào Security:** Giới thiệu công nghệ AWS Security Agent (Frontier Agent) dựa trên Amazon Bedrock để tự động hóa toàn bộ vòng đời bảo mật ứng dụng Web (Design, Code, Pentest).
- **Chuẩn hóa Tư duy Vận hành & Giám sát (Observability):** Chuyển dịch tư duy từ việc chỉ theo dõi hạ tầng truyền thống (SLA/Infrastructure metrics) sang giám sát trải nghiệm thực tế của người dùng (User Experience & Business metrics).
- **Chiến lược Chinh phục Chứng chỉ AWS:** Cung cấp lộ trình ôn luyện chiến lược cho chứng chỉ AWS Certified Cloud Practitioner (CLF-C02) cùng các kỹ thuật làm bài phỏng vấn / thi cử thực tế.

---

### Danh Sách Diễn Giả

- **Thịnh Nguyễn** – *DevOps/DevSecOps/Cloud Engineer, Styl Solutions* (First Cloud AI Journey Member).
- **Nguyễn Huỳnh Sơn** – *Infrastructure Support Engineer, Endava* (Ex-Infrastructure Reliability Engineer at SPS, AWS Student Builder Group HUFLIT Member).
- **Ngô Lê Tấn Huy** – *Presenter / Cloud Engineering Enthusiast*.

---

### Nội Dung Nổi Bật

#### 1. Securing Your Web Apps With AWS Security Agent (Diễn giả: Thịnh Nguyễn)

- **Điểm nghẽn Bảo mật Truyền thống (The Security Bottleneck):**
  - Đánh giá an ninh mạng (Pentest) thủ công tốn thời gian (kéo dài hàng tuần), chi phí đắt đỏ ($5,000 - $20,000/đợt) và kết quả phụ thuộc vào cảm xúc/trình độ kỹ sư.
- **AWS Security Agent (Frontier Agent):**
  - **Autonomous Reasoning:** Sử dụng Amazon Bedrock để tự lập kế hoạch và thực thi nhiệm vụ bảo mật mà không cần con người can thiệp.
  - **Toàn bộ vòng đời (Full Lifecycle):**
    1. *Design Security Review:* Đánh giá file Markdown hoặc code Terraform theo các bộ tiêu chuẩn Managed Packs (PCI DSS, NIST CSF, AWS Well-Architected). Free tier: 200 reviews/tháng.
    2. *Code Security Review:* Tích hợp trực tiếp vào GitHub/GitLab Pull Requests, tự động comment lỗ hổng và đề xuất mã sửa lỗi (Auto-PR Fixes). Free tier: 1,000 PR reviews/tháng.
    3. *Automated Pentesting:* Tự động thực thi chuỗi khai thác đa bước (Multi-step exploit chains như IDOR -> XSS), xác thực như người dùng thực và cung cấp báo cáo Verifiable Findings (bằng chứng khai thác thực tế).
- **Chi phí & Hạn chế thực tế:**
  - *Chi phí:* Pay-as-you-go $50/Task-Hour (So với đợt Pentest $10,000 thì đây là mức giá tối ưu cho doanh nghiệp).
  - *Hạn chế:* Dễ bị chặn bởi cơ chế xác thực phức tạp (MFA, Biometrics, mTLS); khó phát hiện lỗi logic nghiệp vụ phức tạp (Business logic flaws).

---

#### 2. SLA and Monitoring: From SLA to Monitoring what really matters (Diễn giả: Nguyễn Huỳnh Sơn)

- **Định nghĩa SLA & Quản trị Rủi ro (Risk Management):**
  - SLA (Service Level Agreement) là cam kết chính thức về mức độ dịch vụ giữa nhà cung cấp và khách hàng.
  - Giám sát (Monitoring) nằm trong quy trình quản trị rủi ro: *Identify risk -> Monitor signals -> Respond -> Improve*.
- **Khoảng cách giữa "Healthy Infrastructure" và "Happy User":**
  - **Kim tự tháp Giám sát (Monitoring Pyramid):**
    1. *Customer Experience:* User có đăng nhập, mua hàng được không? (Mức cao nhất)
    2. *Business Metrics:* Tỷ lệ đăng nhập thành công, số lượng đơn hàng, doanh thu.
    3. *Application Metrics:* Đô trễ (Latency), Tỷ lệ lỗi (Error rate), Số lượng request.
    4. *Infrastructure Metrics:* CPU, Memory, Disk, Network.
    5. *Cloud Provider:* EC2, RDS, ALB, S3.
  - **Thực tế nghiệt ngã:** Mọi chỉ số hạ tầng báo "Green" (CPU 18%, ALB Target 2/2, Healthcheck /health 200 OK) không đồng nghĩa với trải nghiệm người dùng tốt. Nếu kết nối cơ sở dữ liệu (RDS) thất bại, luồng `/login` trả về lỗi nhưng hệ thống giám sát server vẫn báo bình thường.
- **Lời khuyên vận hành:** *"Everything fails all the time, so plan for failure and nothing fails"* (Dr. Werner Vogels - CTO Amazon). Doanh nghiệp chịu trách nhiệm cho Trải nghiệm khách hàng, không chỉ là độ sẵn sàng của máy chủ.

---

#### 3. Inside The Exam: AWS Cloud Practitioner (Diễn giả: Ngô Lê Tấn Huy)

- **Cấu trúc bài thi AWS Certified Cloud Practitioner (CLF-C02):**
  - Bài thi gồm 65 câu hỏi trắc nghiệm / nhiều lựa chọn, thời lượng 90 phút (người không nói tiếng Anh bản ngữ được xin thêm 30 phút), điểm đạt $\ge 700/1000$.
  - **Phân bổ trọng số 4 Domain:**
    1. *Domain 1: Cloud Concepts (24%)* – Tư duy chuyển đổi số, 6 lợi ích Cloud, AWS WAF, AWS CAF.
    2. *Domain 2: Security and Compliance (30%)* – Shared Responsibility Model, IAM (Least Privilege), Security Groups vs NACLs, AWS Shield/WAF, AWS Artifact.
    3. *Domain 3: Cloud Technology and Services (34%)* – Khái niệm & Use case các dịch vụ Compute, Storage, Database, Networking.
    4. *Domain 4: Billing, Pricing, and Support (12%)* – Các mô hình giá EC2, AWS Cost Explorer, AWS Budgets, các gói Support Plan.
- **Phương pháp ôn luyện & Thi cử hiệu quả:**
  - **Map Keyword Thinking:** Gắn liền tên dịch vụ với 1–2 Từ khóa cốt lõi (ví dụ: *"Decouple/Microservices"* -> SQS; *"Data monetization"* -> Business Perspective in CAF).
  - **Review bài làm sai:** Phân tích kỹ lý do vì sao đáp án A đúng và B, C, D sai để tránh "bẫy" của đề thi.
  - **Kỹ thuật làm bài (Tips & Tricks):** Sử dụng phương pháp loại trừ (Elimination) để loại bỏ các dịch vụ bịa đặt; sử dụng tính năng *"Flag for review"* cho các câu hỏi phân vân; lưu ý các từ khóa quyết định (*NOT, Least cost, Most scalable*).

---

### Những Gì Học Được

#### Tư Duy Bảo Mật & AI (DevSecOps Mindset)

- **Chuyển dịch sang Pentesting tự động:** Tận dụng Generative AI Agents để kiểm tra an ninh hệ thống liên tục trên CI/CD thay vì chờ đến đợt đánh giá định kỳ hàng năm.
- **Kiểm chứng thực tế (Verifiable Proof):** Đánh giá lỗ hổng bảo mật dựa trên bằng chứng khai thác thành công chứ không chỉ dừng lại ở cảnh báo lý thuyết của các công cụ scan tĩnh (SAST/DAST).

#### Tư Duy Giám Sát Chiều Sâu (Observability Mindset)

- **Lấy người dùng làm trung tâm (User-Centric Monitoring):** Xây dựng Dashboard và Metric giám sát dựa trên luồng hành vi thực tế của khách hàng (Login, Checkout, Payment) thay vì chỉ nhìn vào CPU/Memory.
- **Tự động hóa Cảnh báo (Alerting Flow):** Thiết lập luồng từ *Custom Metric -> CloudWatch Alarm -> SNS Topic -> Email/Slack* để phát hiện và xử lý sự cố trước khi khách hàng phàn nàn.

#### Phương Pháp Học & Lấy Chứng Chỉ Cloud

- **Học theo Use Case & Keyword:** Không cần nhớ cấu hình chi tiết, tập trung nắm vững bức tranh tổng thể (Big-picture overview) và mục đích sử dụng dịch vụ.
- **Tự tay thực hành (Hands-on Practice):** Sử dụng AWS Free Tier để trải nghiệm thực tế các dịch vụ cốt lõi, giúp ghi nhớ lâu và trực quan hóa kiến thức lý thuyết.

---

### Ứng Dụng Vào Công Việc & Dự Án

- **Triển khai DevSecOps trong dự án:**
  - Tích hợp công cụ scan code và tự động kiểm tra kiến trúc Infrastructure as Code (Terraform) trên GitHub Actions/GitLab CI.
- **Xây dựng Hệ thống Giám sát toàn diện:**
  - Thiết lập Synthetic Monitoring / Custom Metrics theo dõi tỷ lệ thành công của các API quan trọng (đăng nhập, thanh toán).
  - Cấu hình cảnh báo CloudWatch Alarm kết hợp Amazon SNS gửi thông báo tức thì qua Telegram/Slack khi luồng nghiệp vụ bị gián đoạn.
- **Lập kế hoạch thi lấy chứng chỉ AWS:**
  - Hoàn thành khóa học *Cloud Practitioner Essentials* trên AWS Skill Builder.
  - Ôn luyện Đề thi mẫu, áp dụng phương pháp loại trừ và phân tích bẫy đề thi để tự tin đạt chứng chỉ CLF-C02 trong thời gian ngắn nhất.

---

### Trải Nghiệm & Thu Hoạch Cá Nhân

Tham gia sự kiện ngày 11/7 mang lại nhiều kiến thức thực chiến quý báu, cập nhật những công nghệ bảo mật tiên tiến nhất cũng như chuẩn hóa tư duy vận hành hệ thống Cloud.

#### Học hỏi từ các diễn giả
- **Anh Thịnh Nguyễn:** Mở rộng góc nhìn về việc ứng dụng AI Agents vào thử nghiệm xâm nhập (Pentest) thực tế trên AWS, hiểu rõ ưu điểm và những điểm giới hạn về chi phí/kỹ thuật.
- **Anh Nguyễn Huỳnh Sơn:** Định hình lại tư duy về Observability, hiểu rõ câu nói *"Healthy infrastructure $\neq$ Happy users"* và tầm quan trọng của việc giám sát luồng trải nghiệm khách hàng.
- **Anh Ngô Lê Tấn Huy:** Nắm trọn lộ trình chiến lược, mẹo làm bài thi và cách tư duy từ khóa để chinh phục chứng chỉ AWS Certified Cloud Practitioner một cách hiệu quả nhất.

#### Bài học đắt giá
- **Security-first & Automation:** Bảo mật cần được đưa vào ngay từ khâu thiết kế (Design Review) và mã nguồn (Code Review) thông qua tự động hóa.
- **Chịu trách nhiệm về Trải nghiệm:** Đảm bảo hệ thống hoạt động không dừng lại ở việc server "sống", mà phải đảm bảo người dùng cuối thực hiện thành công giao dịch.
- **Sự chuẩn bị kỹ lưỡng:** Bất kể là vận hành hệ thống hay tham gia kỳ thi chứng chỉ, việc hiểu rõ bản chất và chuẩn bị có chiến lược luôn là yếu tố quyết định thành công.

---

> **Tổng kết:** Sự kiện giúp tôi trang bị cả tư duy kỹ thuật thực chiến (Security AI, Observability) lẫn lộ trình phát triển sự nghiệp cá nhân (AWS Certification), tạo nền tảng vững chắc để phát triển trở thành một Cloud / DevSecOps Engineer chuyên nghiệp.

---

### Hình Ảnh Sự Kiện

![Trình bày về ứng dụng AWS Security Agent trong bảo mật ứng dụng Web](/images/4-EventParticipated/4.2-Event2/event2-1.jpg)

![Trình bày về mô hình SLA và quan điểm Giám sát Observability](/images/4-EventParticipated/4.2-Event2/event2-2.jpg)

![Trình bày chiến lược ôn luyện chứng chỉ AWS Certified Cloud Practitioner](/images/4-EventParticipated/4.2-Event2/event2-3.jpg)

![Hình ảnh lưu niệm tập thể sự kiện AWS Technical Deep Dive & Certification Roadmap](/images/4-EventParticipated/4.2-Event2/event2-4.jpg)

