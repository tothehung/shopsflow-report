---
title: "Event 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Báo cáo tổng kết: “AWS: Enterprise Cloud Architectures and Industry Application”

### Mục Đích Của Sự Kiện

- **Góc nhìn Kiến trúc Doanh nghiệp:** Giới thiệu các mô hình kiến trúc điện toán đám mây chuẩn Enterprise trên AWS (đặc biệt là khung Data Platform DNA) và các nguyên lý thiết kế hệ thống thực tế.
- **Định hướng Sự nghiệp Cloud & AI:** Cung cấp lộ trình phát triển sự nghiệp và tập kỹ năng kỹ thuật cốt lõi cho sinh viên công nghệ trong bối cảnh sự bùng nổ của Điện toán đám mây và Trí tuệ nhân tạo (Generative AI, AI Agents).
- **Gắn kết Học thuật & Doanh nghiệp:** Chuyến Study Tour dành riêng cho sinh viên Swinburne Việt Nam (bao gồm các cơ sở Cần Thơ và TP.HCM) nhằm thu hẹp khoảng cách giữa lý thuyết giảng đường (môn học Kiến trúc Điện toán Đám mây - giáo trình AWS Academy) và nhu cầu tuyển dụng thực tế.
- **Thấu hiểu theo từng Nhóm ngành:** Giới thiệu phương pháp áp dụng các dịch vụ AWS để giải quyết các bài toán kinh doanh cụ thể qua 6 nhóm ngành kinh tế cốt lõi tại Việt Nam.

---

### Danh Sách Diễn Giả & Khách Mời

- **Nguyễn Trần Minh Duy** – *Industry Liaison Officer, Swinburne Vietnam* (Khai mạc & MC sự kiện).
- **Nguyễn Gia Hưng** – *Head of Solution Architect, AWS Vietnam* (Founder của chương trình **First Cloud AI Journey**).
- **Bành Cẩm Vĩnh** – *Data Engineer, Renova Cloud* (AWS Community Builder).
- **Như Trần** – *Account Manager, AWS Vietnam* (Phiên chia sẻ & Thảo luận tương tác).
- **Khang Nguyễn** – *Solution Architect, Cloud Kinetics* (Cựu sinh viên Swinburne Việt Nam - Khóa K3).

---

### Nội Dung Nổi Bật

#### 1. Xu hướng Thị trường Việc làm Cloud tại Việt Nam (Diễn giả: Nguyễn Gia Hưng)

- **Dịch chuyển sang Kiến thức Ngành (Industry-Based):** Nhu cầu tuyển dụng đang dịch chuyển từ các vị trí kỹ thuật thuần túy sang tư duy *Industry-based* (hiểu sâu về nghiệp vụ kinh doanh của 6 nhóm ngành chính: Tài chính & Ngân hàng/Fintech, Bán lẻ & Thương mại điện tử, Sản xuất, Bất động sản, Tiêu dùng/Y tế, và Các ngành mới nổi/Ngành khác).
- **Tăng trưởng Tốc độ & Áp lực Production:** Thị trường đám mây tại Việt Nam đang mở rộng theo cấp số nhân (doanh thu AWS Việt Nam tăng gần 20 lần trong 6 năm). Các doanh nghiệp ưu tiên chiến lược *Cloud-first* và đòi hỏi sinh viên mới ra trường phải có năng lực giải quyết bài toán thực tế ngay lập tức khi vào làm việc.
- **Gia tăng Khoảng cách Kỹ năng & Tác động của AI:** Doanh nghiệp ngày càng giảm sự kiên nhẫn với các vị trí Junior trình độ sơ khởi; thay vào đó, họ yêu cầu ứng viên thành thạo việc sử dụng AI/AI Agent để học nhanh, làm chủ các công nghệ phức tạp (Kubernetes, Cloud-native AI) và thể hiện khả năng chịu áp lực cao.
- **Mô hình Nhân sự Kim tự tháp Ngược:** Tuyển dụng tập trung mạnh vào nhân tài cấp Senior kết hợp với AI Agent, đòi hỏi các nhân sự trẻ phải trưởng thành và đạt năng lực cấp Senior trong khoảng thời gian ngắn nhất có thể.

#### 2. Kiến trúc Data Platform & Khoảng cách giữa Nhà trường và Thực tế (Diễn giả: Bành Cẩm Vĩnh)

- **Mô hình Data Platform DNA Chuẩn hóa:** Hệ thống dữ liệu doanh nghiệp phải tuân thủ kiến trúc cốt lõi 5 trụ cột:
  1. *Ingestion* (Thu thập dữ liệu từ các hệ thống đa nguồn)
  2. *Processing* (Biến đổi & Thực thi pipeline xử lý)
  3. *Storage* (Lưu trữ an toàn và có khả năng mở rộng)
  4. *Governance & Catalog* (Tuân thủ, bảo mật & quản lý quyền truy cập)
  5. *Analytics & Serving* (Báo cáo dashboard, analytics & phục vụ kinh doanh)
- **Đồ án Nhà trường vs. Hệ thống Production Doanh nghiệp:**
  - *Nhà trường:* Dữ liệu sạch, dung lượng nhỏ, yêu cầu rõ ràng, thời hạn thoải mái, không chịu thiệt hại tài chính.
  - *Production Doanh nghiệp:* Dữ liệu bẩn/thiếu từ nhiều nguồn, yêu cầu kinh doanh liên tục thay đổi, thời hạn gấp rút gắn liền với doanh thu, và sự cố gián đoạn hệ thống gây ra thiệt hại trực tiếp về kinh doanh.
- **Tư duy Kiến trúc & Sự Đánh đổi (Trade-offs):** Làm chủ các nguyên lý kiến trúc nền tảng thay vì chạy theo công cụ; liên tục đánh giá sự đánh đổi giữa Chi phí, Tốc độ Triển khai và Độ Tin cậy Hệ thống.

#### 3. Kỹ năng Mềm, Giao tiếp & Nắm bắt Cơ hội (Diễn giả: Như Trần)

- **Vượt qua Nỗi sợ Thất bại & Sợ bị Đánh giá:** Phân biệt đúng giữa nỗi sợ hậu quả và nỗi sợ bị người khác đánh giá để bứt phá giới hạn; coi thời gian đại học là "trả tiền để được thất bại an toàn" và môi trường làm việc là "được nhận lương để thực thi chính xác".
- **Giao tiếp như một Kỹ năng Kỹ thuật:** Giao tiếp hiệu quả với ban lãnh đạo, khách hàng và các đội ngũ liên phòng ban (làm cầu nối giữa thực thi kỹ thuật và chiến lược kinh doanh).
- **Tạo ra Cơ hội (Đại dương Đỏ vs. Đại dương Xanh):** Tìm kiếm các cơ hội ẩn giấu bằng cách tăng cường Độ hiện diện (Visibility), duy trì Sự kiên định (Perseverance), và cam kết Học tập Suốt đời (Lifelong Learning).

#### 4. Kỹ năng, Tư duy & Sự Thích ứng trong Kỷ nguyên AI (Diễn giả: Khang Nguyễn)

- **Làm chủ Công cụ AI ("Bạn không thể úp bô sự hiểu biết của chính mình"):** AI giúp tăng tốc công việc và giải phóng thời gian, nhưng kỹ sư bắt buộc phải hiểu rõ bản chất kỹ thuật cốt lõi để xác minh tính đúng đắn của đầu ra do AI tạo ra.
- **Thái độ & Độ sâu Trải nghiệm:** Tuyển dụng nhân sự đầu vào ưu tiên Thái độ và Tiềm năng Phát triển; tập trung tích lũy Độ sâu Trải nghiệm Thực tế thay vì chỉ đếm Số năm Kinh nghiệm.
- **Mô hình Cân bằng Công việc 3 Yếu tố:** Đánh giá các lựa chọn sự nghiệp dựa trên 3 yếu tố cốt lõi: Đam mê - Trách nhiệm - Lợi ích Tổng thể (Lương thưởng, Mạng lưới quan hệ, Kiến thức, Trải nghiệm, Sự tăng trưởng).

---

### Những Bài Học Cốt Lõi (Key Takeaways)

#### Tư Duy Thiết Kế & Vận Hành

- **Ownership (Tinh thần làm chủ):** Chịu trách nhiệm cá nhân hoàn toàn đối với các giải pháp kỹ thuật đề xuất và kiến trúc đã triển khai.
- **Architecture Thinking (Tư duy kiến trúc):** Nhìn nhận hệ thống toàn cục từ đầu đến cuối (end-to-end); thiết kế cho khả năng Mở rộng (Scalability), Độ tin cậy (Reliability), Tính sẵn sàng cao (HA), và Bảo mật (Security).
- **Communication & Alignment (Giao tiếp & Đồng bộ):** Coi giao tiếp là kỹ năng kỹ thuật bắt buộc để làm rõ yêu cầu kinh doanh và đồng bộ quy trình làm việc giữa các phòng ban.

#### Kiến Trúc Kỹ Thuật

- **Môi trường Startup vs. Doanh nghiệp Enterprise:**
  - *Startup:* Xây dựng nhanh, tối ưu hóa nguồn lực hạn chế, linh hoạt thích ứng.
  - *Enterprise:* Quy trình chặt chẽ, hạ tầng quy mô lớn đa vùng (Multi-AZ/Hybrid), chuẩn hóa bảo mật và tuân thủ cao.
- **Data Platform DNA & Well-Architected Framework:** Thành thạo mô hình dòng chảy dữ liệu 5 tầng và tận dụng AWS Well-Architected Framework để đánh giá và tối ưu hóa khối lượng công việc trên đám mây.

#### Chiến Lược Phát Triển Bản Thân

- **Dự án Side Project Đạt chuẩn Production:** Xây dựng các dự án giải quyết bài toán thực tế thuộc 6 nhóm ngành kinh tế chính, có tài liệu hướng dẫn đầy đủ, tự động hóa triển khai và tuân thủ các tiêu chuẩn bảo mật.
- **Nền tảng Kỹ thuật Cốt lõi Vững chắc:** Nắm vững Mạng máy tính (Networking), Hệ điều hành (OS), Cấu trúc dữ liệu & Giải thuật, Cơ sở dữ liệu Phân tán, và RESTful APIs.
- **Công thức Giá trị Sự nghiệp:** $\text{Kết quả} = \text{Năng lực} \times \text{Độ hiện diện (Visibility)} \times \text{Tính kiên định (Consistency)}$.

---

### Ứng Dụng Vào Dự Án & Phát Triển Bản Thân

- **Củng cố Nền tảng Cloud:** Hệ thống hóa kiến thức các dịch vụ AWS cốt lõi (VPC, EC2, S3, IAM, Redshift, Glue, ECS/EKS) bám sát các tiêu chuẩn của AWS Academy.
- **Tối ưu hóa Quy trình Phát triển:** Sử dụng các công cụ AI (Amazon Q Developer, Kiro) một cách có trách nhiệm để thực hiện code review, kiểm tra lỗ hổng bảo mật và chuẩn hóa tài liệu kiến trúc.
- **Thực hành Dữ liệu Thực tế & E-Commerce:** Triển khai các dự án tập trung vào xử lý dữ liệu quy mô lớn, tính đồng nhất dữ liệu, quản lý quyền truy cập chi tiết (Governance), và phân tích hành vi người dùng.
- **Kết nối Cộng đồng:** Tích cực đóng góp và kết nối trong cộng đồng *AWS First Cloud AI Journey* cùng với AWS Study Group để học hỏi kinh nghiệm thực tế từ các Kiến trúc sư Đám mây (Cloud Architects) và AWS Community Builders.

---

### Trải Nghiệm & Thu Hoạch Cá Nhân

Tham dự sự kiện Study Tour **“AWS: Enterprise Cloud Architectures and Industry Application”** tại văn phòng AWS Việt Nam mang lại nhiều góc nhìn thực tiễn sâu sắc và nguồn cảm hứng mạnh mẽ cho bản thân.

#### Góc nhìn từ Diễn giả
- Lắng nghe những lời khuyên thực tế, truyền cảm hứng từ anh Nguyễn Trần Minh Duy, anh Nguyễn Gia Hưng, anh Bành Cẩm Vĩnh, chị Như Trần và anh Khang Nguyễn về thực tế tuyển dụng, tư duy làm việc và sự phát triển sự nghiệp.
- Nhận diện rõ ràng lộ trình phát triển bản thân từ sinh viên đại học trở thành Data Engineer, Solution Architect, và Account Manager.

#### Hướng dẫn Kỹ thuật Thực tế & Chiến lược
- Làm chủ mô hình Data Platform DNA cấp doanh nghiệp và các nguyên lý thiết kế hạ tầng Enterprise.
- Học cách áp dụng AI hiệu quả để nâng cao năng suất trong khi vẫn giữ vững sự chủ động đối với kiến thức nghiệp vụ cốt lõi.

#### Những Bài học Không Phai mờ Theo Thời gian
- **Tinh thần Bền bỉ (Resilience):** Duy trì tư duy phát triển, coi thất bại là cơ hội học hỏi và kiên định với các mục tiêu dài hạn.
- **Cân bằng Kỹ năng Kỹ thuật & Kinh doanh:** Kết hợp giữa chuyên môn kỹ thuật sâu rộng với sự thấu hiểu nghiệp vụ kinh doanh chính là chìa khóa để tạo ra giá trị vượt trội.
- **Học tập Suốt đời (Lifelong Learning):** Các công cụ công nghệ sẽ liên tục thay đổi, nhưng tư duy kiến trúc nền tảng và tinh thần học hỏi không ngừng sẽ luôn là tài sản cốt lõi trường tồn.

---

> **Tóm tắt:** Sự kiện không chỉ trang bị kiến thức kiến trúc kỹ thuật chuẩn doanh nghiệp mà còn tái định hình tư duy sự nghiệp của em, giúp em chuẩn bị hiệu quả cho chặng đường trở thành một Kỹ sư Cloud / Data / DevOps chuyên nghiệp trong tương lai.

---

### Hình Ảnh Sự Kiện

![Tổng quan sự kiện Enterprise Cloud Architectures & Industry Application](/images/4-EventParticipated/4.1-Event1/event1-1.jpg)

![Hình ảnh thu hoạch cá nhân tại sự kiện Enterprise Cloud Architectures](/images/4-EventParticipated/4.1-Event1/event1-2.jpg)
