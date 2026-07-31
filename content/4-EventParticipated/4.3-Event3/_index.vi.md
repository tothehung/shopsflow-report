---
title: "Event 3"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Bài thu hoạch “FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!”

### Mục Đích Của Sự Kiện

- **Chia sẻ kinh nghiệm thực chiến từ Hackathon:** Đúc kết những trải nghiệm, khó khăn và bài học xương máu trong quá trình thiết kế, phát triển và thuyết trình (pitching) các sản phẩm ứng dụng Agentic AI trên hạ tầng AWS trong 24 giờ liên tục.
- **Tư duy thiết kế hệ thống Agentic AI (Multi-Agent Architectures):** Giới thiệu kiến trúc thực tế của các giải pháp đoạt giải: từ AI đàm thoại gọi món (Conversational Ordering Agent), Phân tích Tín hiệu Đối thủ (Competitive Intelligence System), Trợ lý Kiến trúc sư đám mây (Cloud Architecture Assistant) cho đến Hệ thống Giám sát Video/Phát hiện Rửa tiền tự động (Computer Vision & Anti-Money Laundering Agents).
- **Thu hẹp khoảng cách giữa Công nghệ & Bài toán Kinh doanh:** Nhấn mạnh tầm quan trọng của việc hiểu rõ nỗi đau khách hàng (Pain points/Business Requirements), khả năng kiểm soát chi phí (Cost Efficiency), độ tin cậy của mô hình (Reducing Hallucinations) và nguyên tắc luôn có con người trong luồng xử lý (Human-in-the-loop).
- **Thúc đẩy tinh thần Học tập & Làm việc nhóm (Teamwork & Lifelong Learning):** Truyền cảm hứng cho sinh viên công nghệ tự tin tham gia các sân chơi thực chiến, xây dựng mạng lưới kết nối (Networking) và ứng dụng các dịch vụ Cloud / AI mới nhất vào thực tế.

---

### Danh Sách Diễn Giả & Các Đội Thi Chia Sẻ

- **Mr. Joseph Marazota** – *Head of Technology, Asian Region* (Phát biểu định hướng & Khai mạc).
- **Mr. Nguyễn Gia Hưng** – *Head of Solution Architect, AWS Việt Nam* (Founder chương trình **First Cloud AI Journey**).
- **Team One Team (Giải Nhất AWS Track):** Giải pháp *KFC Voice/Conversational Agent* – Hệ thống AI đàm thoại đặt món đa kênh (Zalo/WhatsApp) không cần chuyển đổi ứng dụng.
- **Team Signal Scout / Hành Signal C (Giải Nhì AWS Track - Sinh viên FPT):** Giải pháp *Multi-Agent Corporate Intelligence* – Hệ thống thu thập, phân tích tín hiệu đối thủ và dự báo tác động chiến lược kinh doanh.
- **Team Plan:** Giải pháp *SA Professional AI Native Assistant* – Trợ lý AI tự động phân tích tài liệu nghiệp vụ, tạo sơ đồ kiến trúc (Draw.io), xuất bảng giá và sinh mã Terraform/CloudFormation.
- **Team 3K (Sinh viên FPT):** Giải pháp *Shepherd (Small Human Fluctuation Federation Response)* – Hệ thống giám sát luồng người theo thời gian thực kết hợp AI Agent điều phối đám đông qua camera kết nối AWS Kinesis & Bedrock.
- **Team Six Pillars:** Giải pháp *Adaptive Workflow Engine for Anti-Money Laundering (AML)* – Hệ thống Multi-Agent phát hiện, điều tra và hỗ trợ phòng chống rửa tiền cho ngân hàng và các sàn giao dịch tài chính.

![Hình ảnh sự kiện FCAJ x Agentic AI Build Week](/images/4-EventParticipated/4.3-Event3/event3.jpg)

---

### Nội Dung Nổi Bật

#### 1. Thông điệp Khai mạc & Tư duy Đổi mới trong Kỷ nguyên AI (Mr. Joseph Marazota)

- **Sự thay đổi về Mental Model:** Kỷ nguyên AI/Agent rút ngắn chu kỳ phát hành phần mềm từ hàng quý (hàng chục năm trước) xuống còn hàng phút nhờ tự động hóa hoàn toàn.
- **Thách thức & Cơ hội của thế hệ trẻ:** Khuyên các kỹ sư trẻ mạnh dạn thách thức các quy chuẩn cũ ("Challenge them every single day"). AI và Robot (Amazon đang vận hành hơn 1 triệu robot) chỉ là công cụ cứng, giá trị sáng tạo thực sự nằm ở dữ liệu, tư duy kiến trúc và quyết định của con người trong luồng (*Human-in-the-loop*).
- **Tinh thần Lifelong Learning:** Liên tục học hỏi và áp dụng ngay những tri thức mới vào thực tế hàng ngày để đạt được thành công bền vững trong sự nghiệp.

#### 2. Giải pháp "AI-Powered Conversational Ordering Agent" (Team One Team - Quán quân AWS Track)

- **Bài toán thực tế:** Khách hàng gặp rào cản lớn (friction) khi phải tải ứng dụng mới, tạo tài khoản và thao tác trên menu phức tạp để đặt đồ ăn. Bài học từ McDonald's cho thấy AI thoại dễ bị hiện tượng "ảo giác" (hallucinations) nếu không xác nhận kỹ đơn hàng.
- **Giải pháp:** Xây dựng Agent đàm thoại đa kênh trực tiếp trên Zalo/WhatsApp. Đơn hàng được xử lý ngay trong cuộc trò chuyện bằng cách phối hợp giữa **Agentic Core** (trên Amazon Bedrock) và công cụ bóc tách dữ liệu (TinyFish/Scraper).
- **Kiến trúc & Tối ưu Chi phí:**
  - Sử dụng *Agent Core* có bộ nhớ context (*Agent Memory*) giúp ghi nhớ lịch sử và thói quen đặt hàng của người dùng.
  - Tối ưu chi phí vận hành đạt khoảng **$0.006/đơn hàng** (chi phí hạ tầng khoảng $88/tháng cho 500 đơn/ngày), phản hồi End-to-End cực nhanh từ 3–5 giây nhờ cắt giảm các lớp trung gian không cần thiết.

#### 3. Giải pháp "Multi-Agent Corporate Intelligence" (Team Signal Scout - Á quân AWS Track)

- **Bài toán nghiệp vụ:** Các tập đoàn lớn tốn nhiều thời gian và nguồn lực để theo dõi, thu thập các tín hiệu rời rạc của đối thủ cạnh tranh (báo cáo tài chính, tài liệu cổ đông, tin tức) nhằm điều chỉnh chiến lược kinh doanh.
- **Kiến trúc Multi-Agent & Value Canvas:**
  - *Crawler Subagent:* Tự động thu thập dữ liệu bằng Apify (trang tĩnh) hoặc TinyFish (trang động/vượt Login wall), lọc qua code thuần để giảm chi phí Token và ngăn chặn Prompt Injection.
  - *Analysis Subagent:* Sử dụng Bedrock Guardrails kiểm soát input/output, gửi dữ liệu sang LangFuse đánh giá chất lượng; nếu đạt điểm cao sẽ lưu vào Amazon S3 & DynamoDB metadata, nếu điểm thấp sẽ kích hoạt luồng chạy lại (tối đa 2 lần) trước khi gắn tag yêu cầu con người duyệt.
- **Bài học về Chi phí & Khả năng Tuân thủ:** Đội thi đề xuất phương án chuyển dịch từ các dịch vụ bên thứ ba (Apify/TinyFish/LangFuse) sang các công cụ AWS Native (AWS Web/Browser Tools, Bedrock) để đảm bảo an toàn dữ liệu doanh nghiệp (Data Residency) và giảm chi phí từ $130/tháng xuống mức tối ưu.

#### 4. Giải pháp "SA Professional AI Native Assistant" (Team Plan)

- **Nỗi đau nghề nghiệp:** Các Solution Architect (SA) mất nhiều ngày để chuyển đổi yêu cầu/policy kinh doanh của khách hàng thành sơ đồ kiến trúc, bảng tính chi phí và mã khởi tạo hạ tầng (IaC).
- **Kiến trúc & Tính năng Cốt lõi:**
  - Phân tích yêu cầu bằng ngôn ngữ tự nhiên hoặc tài liệu nghiệp vụ doanh nghiệp.
  - Tự động sinh sơ đồ kiến trúc chuẩn hóa trên Draw.io (sử dụng đúng bộ icon chính thức của AWS), cho phép SA tùy chỉnh trực tiếp.
  - Tự động tính toán bảng giá dịch vụ và sinh mã triển khai (Terraform/CloudFormation) tuân thủ các quy chuẩn thiết kế nội bộ (Internal Best Practices).
- **Tư duy Kỹ thuật (High Engineering):** Tập trung vào quản lý Context, Memory và thiết lập cơ chế kiểm tra lỗi (*Validation/Blacklist Filter*) để chặn các dịch vụ không mong muốn và hạn chế sự sai lệch của AI giữa các lần tạo code.

#### 5. Trực quan hóa Đám đông "Shepherd System" (Team 3K) & Điều tra Rửa tiền "AML Engine" (Team Six Pillars)

- **Team 3K - Camera AI & Điều phối Đám đông:**
  - Kết nối hệ thống Camera bằng **Amazon Kinesis Video Streams**, xử lý phân tích hình ảnh nhận diện người dùng qua YOLOv8/ByteTrack chạy trên AWS Fargate.
  - Tích hợp **Amazon Bedrock Agent** có bộ nhớ lịch sử để tự động phát hiện các vùng ùn tắc (zone congestion) tại sân bay/siêu thị, đưa ra cảnh báo và đề xuất phương án điều phối nhân sự hỗ trợ theo thời gian thực (*Autonomous Monitor & Operator Copilot*).
- **Team Six Pillars - Phòng chống Rửa tiền Đa tầng (AML System):**
  - Giải quyết bài toán tỷ lệ cảnh báo sai quá cao (90–95% False Positives) gây tốn kém chi phí review thủ công ($20–$25/case) trong ngành Ngân hàng/Fintech.
  - **Kiến trúc 3 Layer:**
    1. *Layer 1 (Fast Detection):* Dùng Kinesis Data Stream + XGBoost Model phân loại nhanh giao dịch với chi phí thấp.
    2. *Layer 2 (Multi-Agent Investigation):* Sử dụng AWS Step Functions điều phối Orchestrator Agent và 3 Sub-agents (KYC Profile Check, Money Flow Check, Sanction Check) kết hợp Amazon OpenSearch Vector RAG để tổng hợp tài liệu bằng chứng (*Evidence File*).
    3. *Layer 3 (Case Management & Enterprise Trust):* Đưa dữ liệu qua hai tầng LLM Judge (đánh giá chéo chống ảo giác) và Bedrock Guardrails; ghi nhận vết suy luận (*Reasoning Trace*) để con người thẩm định cuối cùng trên Dashboard React/Amplify.

---

### Những Gì Học Được

#### Tư Duy Thiết Kế Kiến Trúc Agentic AI (Agentic Engineering)

- **Mô hình Multi-Agent & Phân công Trách nhiệm:** Không dùng một LLM duy nhất xử lý toàn bộ bài toán phức tạp; cần chia nhỏ thành các Sub-agents đảm nhận nhiệm vụ chuyên biệt (Crawling, Analysis, Profiling, Decision) dưới sự điều phối của một Orchestrator Agent.
- **Giảm thiểu Ảo giác (Mitigating Hallucinations):** Kết hợp kiểm tra hai lớp (LLM-as-a-Judge), thiết lập quy tắc cứng (Rule-based Validation/Guardrails) và bắt buộc lưu lại vết suy luận (*Reasoning Log*) để phục vụ công tác kiểm toán (Audit).
- **Human-in-the-Loop (Con người giữ quyền quyết định):** Trong các lĩnh vực nhạy cảm (Tài chính, Ngân hàng, Bảo mật), Agent AI đóng vai trò là "cánh tay hỗ trợ" tăng tốc độ xử lý từ vài ngày xuống vài phút, còn con người trực tiếp đưa ra quyết định duyệt cuối cùng (*Escalate/Approve*).

#### Kỹ Thuật Lập Trình & Tối Ưu Chi Phí Đám Mây

- **Tối ưu hóa Token & Tốc độ (Latency):** Xử lý lọc dữ liệu thô bằng mã nguồn truyền thống (Code-based ETL) trước khi đưa vào LLM để tiết kiệm Token và tránh nguy cơ bị tấn công Prompt Injection.
- **Ưu tiên AWS Native Services:** Tận dụng tối đa hệ sinh thái AWS (Bedrock, Step Functions, DynamoDB, Lambda, Kinesis, Amplify) thay vì phụ thuộc quá nhiều vào công cụ của bên thứ ba để đảm bảo khả năng mở rộng, giảm chi phí và tuân thủ an toàn thông tin (Compliance/Data Residency).

#### Kỹ Năng Thực Chiến Hackathon & Phát Triển Sự Nghiệp

- **Xác định đúng Phạm vi Dự án (Scope Management):** Tập trung xây dựng sản phẩm khả thi tối thiểu (MVP/POC) giải quyết đúng "nỗi đau" cụ thể thay vì mở rộng tính năng quá đà dẫn đến vỡ tiến độ.
- **Tinh thần Teamwork & Hạ thấp "Cái Tôi":** Lắng nghe phản biện chuyên môn, phân chia công việc rõ ràng theo đúng thế mạnh (AI/SE, Backend, Business, Pitching) và giữ vững tâm lý bình tĩnh trước áp lực thời gian.
- **Trải nghiệm là Giá trị Tối thượng:** Tham gia Hackathon không chỉ vì giải thưởng mà còn là cơ hội để thử nghiệm công nghệ mới, dũng cảm mắc sai lầm, học từ thực tế và mở rộng mạng lưới kết nối cá nhân (Networking).

---

### Ứng Dụng Vào Công Việc & Dự Án

- **Ứng dụng Agentic AI vào bài toán thực tế:** Áp dụng mô hình thiết kế Multi-Agent (Bedrock Agents & Step Functions) để tự động hóa các quy trình nghiệp vụ phức tạp trong dự án cá nhân/doanh nghiệp.
- **Tích hợp Quy trình RAG & Vector Database:** Xây dựng hệ thống truy vấn tri thức doanh nghiệp kết hợp Amazon OpenSearch Service và Bedrock Knowledge Bases nhằm cung cấp thông tin chính xác, hạn chế tối đa ảo giác.
- **Rèn luyện Quy trình Chuẩn bị Hackathon:** Lên kế hoạch chi tiết từ bước Brainstorm ý tưởng, phân chia Task, vẽ sơ đồ kiến trúc hệ thống, chuẩn bị tài liệu đến việc luyện tập thuyết trình (Pitching) tập trung vào Value Proposition và Business Impact.
- **Xây dựng Mạng lưới Kết nối (Networking):** Tích cực tham gia các sân chơi Hackathon (như Agentic AI Build Week) và sinh hoạt thường xuyên tại cộng đồng *AWS First Cloud AI Journey* để giao lưu với các Chuyên gia, tuyển dụng và các Bạn học đồng điệu.

---

### Trải Nghiệm & Thu Hoạch Cá Nhân

Theo dõi và học hỏi từ sự kiện **“FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!”** mang lại cho tôi những bài học thực chiến vô cùng đắt giá và nguồn năng lượng sục sôi về tinh thần làm chủ công nghệ.

#### Lắng nghe chia sẻ từ các Chuyên gia & Đội thi
- Thấu hiểu định hướng tầm nhìn từ Mr. Joseph Marazota về tốc độ thay đổi kỷ nguyên AI và vai trò quyết định của thế hệ trẻ trong việc định hình lại toàn bộ các ngành công nghiệp.
- Lắng nghe những câu chuyện "vừa học, vừa làm, vừa thức xuyên đêm" chân thực từ 5 đội thi: từ việc gặp sự cố đè file `.env` lên GitHub, đường truyền mạng lag khi demo camera, cho đến niềm vui vỡ òa khi sản phẩm chạy thành công và nhận được sự đánh giá cao từ Ban giám khảo.

#### Trải nghiệm tư duy Kỹ thuật & Kinh doanh
- Nhận ra rằng sản phẩm chiến thắng không chỉ nằm ở kỹ thuật phức tạp hay sơ đồ kiến trúc hoành tráng, mà nằm ở **70% giá trị bài toán nghiệp vụ** mà giải pháp đó giải quyết được cho doanh nghiệp với chi phí tối ưu nhất.
- Học được cách quản trị cảm xúc, vượt qua áp lực thời gian và biến các ý tưởng "nghe có vẻ điên rồ" thành các Demo chạy được trên môi trường Production.

#### Bài học đắt giá
- **Show Up - Hãy cứ dũng cảm dấn thân:** Đừng chờ đợi đến khi "đủ trình độ" mới tham gia; hãy cứ đăng ký Hackathon để quăng mình vào môi trường thực chiến và học hỏi nhanh nhất.
- **Build - Tập trung vào Giá trị Thực tế:** Xây dựng sản phẩm giải quyết đúng Pain Point, làm chủ nền tảng cốt lõi và luôn coi công nghệ là phương tiện phục vụ con người.
- **Pitch & Win - Bình tĩnh và Tự tin:** Trình bày giải pháp bằng sự am hiểu sâu sắc, thể hiện tinh thần học hỏi, đón nhận phản hồi từ Chuyên gia và trân trọng từng khoảnh khắc trải nghiệm cùng đồng đội.

---
