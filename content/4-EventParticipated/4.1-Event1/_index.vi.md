---
title: "Event 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch “AWS: Enterprise Cloud Architectures and Industry Application”

### Mục Đích Của Sự Kiện

- **Chia sẻ kiến trúc doanh nghiệp:** Giới thiệu các mô hình kiến trúc điện toán đám mây chuẩn Enterprise trên AWS và quy chuẩn thiết kế hệ thống thực tế.
- **Định hướng sự nghiệp Cloud & AI:** Tư vấn lộ trình phát triển bản thân, kỹ năng cốt lõi cho sinh viên công nghệ trong làn sóng Điện toán đám mây và Trí tuệ nhân tạo.
- **Gắn kết Học thuật & Doanh nghiệp:** Thu hẹp khoảng cách giữa lý thuyết giảng đường và nhu cầu tuyển dụng thực tế từ các doanh nghiệp giải pháp hàng đầu.
- **Ứng dụng thực tiễn:** Giới thiệu phương pháp áp dụng các dịch vụ AWS để giải quyết bài toán kinh doanh cụ thể theo từng nhóm ngành.

---

### Danh Sách Diễn Giả

- **Nguyễn Gia Hưng** – *Head of Solution Architect, AWS Việt Nam* (Founder của chương trình **First Cloud AI Journey**).
- **Bành Cẩm Vĩnh** – *Data Engineer, Renova Cloud* (AWS Community Builder).
- **Khang Nguyễn** – *Solution Architect, Cloud Kinetics* (Cựu sinh viên Swinburne).

---

### Nội Dung Nổi Bật

#### 1. Thị trường việc làm và xu hướng Cloud tại Việt Nam

- **Dịch chuyển sang kiến thức ngành (Industry-base):** Xu hướng tuyển dụng hiện nay dịch chuyển rõ rệt từ việc chỉ quan tâm đến *Role* thuần kỹ thuật (Developer, SysAdmin) sang tư duy *Industry-base* (hiểu nghiệp vụ ngành Tài chính - Ngân hàng, Bán lẻ E-commerce, Sản xuất...).
- **Tốc độ tăng trưởng và áp lực Production:** Thị trường Cloud tại Việt Nam phát triển bùng nổ. Doanh nghiệp đòi hỏi kỹ sư mới ra trường phải có khả năng tiếp cận và giải quyết bài toán thực tế ngay lập tức, không còn khoảng thời gian "cầm tay chỉ việc" kéo dài.
- **Gia tăng khoảng cách kỹ năng (Skill Gap):** Doanh nghiệp giảm sự kiên nhẫn đối với các kỹ năng sơ khởi (demo/toy projects); thay vào đó yêu cầu sự linh hoạt, khả năng chịu áp lực và thấu hiểu sâu sắc quy trình nghiệp vụ kinh doanh.

#### 2. Kiến trúc & Ứng dụng thực tế

- **Framework DNA chuẩn hóa:** Mọi hệ thống dữ liệu doanh nghiệp dù thay đổi công cụ nào cũng phải tuân thủ mô hình kiến trúc 5 tầng cốt lõi:
  1. *Ingestion* (Thu thập dữ liệu)
  2. *Processing* (Xử lý & Biến đổi)
  3. *Storage* (Lưu trữ an toàn)
  4. *Governance* (Quản trị & Bảo mật)
  5. *Analytics / Serving* (Phân tích & Phục vụ business)
- **Tư duy Business-first:** Công nghệ chỉ là phương tiện. Giá trị cốt lõi của Kiến trúc sư giải pháp nằm ở việc thấu hiểu nỗi đau (pain points) của khách hàng và tối ưu hóa chi phí vận hành (ROI).
- **AI & Automation trong thiết kế kiến trúc:** Tận dụng công nghệ Generative AI để tự động hóa review mã nguồn, tối ưu hóa sơ đồ kiến trúc. Tuy nhiên, con người vẫn giữ vai trò quyết định trong việc thiết kế luồng quy trình (workflow) và đảm bảo tính đúng đắn về mặt kinh doanh.

---

### Những Gì Học Được

#### Tư Duy Thiết Kế (Design Mindset)

- **Ownership (Tinh thần làm chủ):** Kỹ sư cần chịu trách nhiệm hoàn toàn với những công nghệ, kiến trúc mà mình lựa chọn và xây dựng.
- **Architecture Thinking (Tư duy kiến trúc):** Nâng cao khả năng nhìn nhận hệ thống toàn cục (end-to-end), đánh giá tính sẵn sàng cao (HA), bảo mật và khả năng mở rộng thay vì chỉ tập trung viết code đơn thuần.
- **Communication (Kỹ năng giao tiếp chuyên môn):** Truyền đạt giải pháp kỹ thuật phức tạp thành ngôn ngữ kinh doanh dễ hiểu là kỹ năng kỹ thuật bắt buộc, không đơn thuần là kỹ năng mềm.

#### Kiến Trúc Kỹ Thuật (Technical Architecture)

- **Startup vs. Enterprise Environment:** Phân biệt rõ sự khác biệt giữa môi trường *Startup* (tự build nhanh từ đầu, linh hoạt) và *Enterprise* (quy trình chặt chẽ, phối hợp đa phòng ban, tuân thủ tiêu chuẩn an toàn thông tin).
- **Trade-off Analysis:** Thành thạo phương pháp đánh giá sự đánh đổi (trade-offs) giữa chi phí, hiệu năng, độ phức tạp và tính bảo mật khi lựa chọn các dịch vụ AWS.

#### Chiến Lược Phát Triển Bản Thân

- **Production-Ready Side Projects:** Xây dựng các dự án cá nhân không chỉ dừng lại ở mức demo mà phải đạt chuẩn doanh nghiệp: bảo mật (Security), sẵn sàng mở rộng (Scalability), và có sẵn script tự động hóa triển khai.
- **Bản chất vững vàng (Core Fundamentals):** Nắm chắc kiến thức nền tảng về mạng (Networking), hệ điều hành (OS), cơ sở dữ liệu và bảo mật thay vì chạy theo các công cụ/framework thay đổi liên tục.

---

### Ứng Dụng Vào Công Việc & Dự Án

- **Định hướng học tập:** Tập trung củng cố kiến thức nền tảng về hạ tầng đám mây (Cloud Architecture Fundamentals) trên AWS trước khi đi sâu vào các dịch vụ chuyên biệt.
- **Phát triển Dự án Shopsflow:** Tận dụng các công cụ AI (như Amazon Q Developer, Kiro) để kiểm tra bảo mật mã nguồn, tối ưu hóa file cấu hình Docker, Terraform và chuẩn hóa tài liệu dự án.
- **Rèn luyện tư duy thực chiến:** Triển khai project theo hướng giải quyết bài toán thương mại điện tử thực tế (xử lý đơn hàng bất đồng bộ, chống overselling, lưu trữ đa tầng).
- **Kết nối cộng đồng:** Tích cực tham gia mạng lưới *First Cloud AI Journey* để giao lưu, học hỏi kinh nghiệm thực chiến từ các Cloud Architects và AWS Community Builders.

---

### Trải Nghiệm & Thu Hoạch Cá Nhân

Tham gia sự kiện **“AWS: Enterprise Cloud Architectures and Industry Application”** mang lại góc nhìn thực tế và truyền cảm hứng mạnh mẽ về hành trình phát triển sự nghiệp Cloud tại Việt Nam.

#### Học hỏi từ các diễn giả
- Lắng nghe những chia sẻ thẳng thắn, giàu nhiệt huyết từ anh Nguyễn Gia Hưng và các diễn giả về bức tranh tuyển dụng thực tế và tư duy cần có của một kỹ sư Cloud trẻ.
- Thấy được lộ trình phát triển nghề nghiệp thực tế từ Data Engineer lên Solution Architect thông qua những câu chuyện làm nghề chân thực.

#### Trải nghiệm kỹ thuật thực tế
- Tiếp cận mô hình chuẩn hóa Data Platform DNA và phương pháp thiết kế hạ tầng Enterprise Multi-AZ.
- Học cách đưa AI vào quy trình tự động hóa phát triển phần mềm mà vẫn làm chủ được quy trình nghiệp vụ cốt lõi.

#### Bài học bài học đắt giá
- **Kiên trì và Bền bỉ (Resilience):** Tinh thần vượt khó và khả năng chịu áp lực là yếu tố quyết định sự tăng trưởng bền vững trong môi trường doanh nghiệp.
- **Cân bằng Kỹ thuật & Nghiệp vụ:** Sự kết hợp hài hòa giữa năng lực kỹ thuật vững vàng và sự am hiểu bài toán kinh doanh là chìa khóa để tạo ra giá trị khác biệt.
- **Học tập suốt đời (Continuous Learning):** Công nghệ và công cụ sẽ luôn thay đổi, nhưng tư duy kiến trúc nền tảng và tinh thần học hỏi liên tục chính là giá trị cốt lõi trường tồn.

---

> **Tổng kết:** Sự kiện không chỉ trang bị kiến thức kiến trúc chuẩn doanh nghiệp mà còn định hình lại tư duy nghề nghiệp, giúp tôi tự tin và chuẩn bị tốt nhất cho hành trình trở thành một Cloud/DevOps Engineer chuyên nghiệp.
