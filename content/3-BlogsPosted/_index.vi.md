---
title: "Các bài blogs đã đăng"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Trang này tổng hợp các bài viết chia sẻ kinh nghiệm kỹ thuật thực chiến và kiến thức chuyên sâu trên đám mây AWS đã được đăng tải trên Cộng đồng [AWS Study Group FCJ](https://www.facebook.com/groups/awsstudygroupfcj).

---

### [Blog 1 - 3 Chi tiết "ngách" trên AWS ít ai nói cho bạn biết, nhưng đụng vào là dính sự cố](3.1-Blog1/)
Tổng hợp 3 chi tiết kỹ thuật nhỏ trên AWS ít khi xuất hiện trên slide bài giảng nhưng lại là nguyên nhân gây tốn tiền vô lý hoặc tốn nhiều ngày debug:
* **S3 Incomplete Multipart Uploads**: Tiền vẫn mất hàng tháng nhưng file dở dang không hiển thị trên S3 Console.
* **Bẫy IMDSv2 Hop Limit**: Mất quyền AWS SDK khi chạy ứng dụng bên trong Docker Container do mặc định Hop Limit = 1.
* **Thư mục `/tmp` của AWS Lambda**: File tạm bị tồn đọng giữa các lần gọi do cơ chế Warm Start container.

---

### [Blog 2 - Những "cái bẫy" ẩn kỹ trong AWS mà tài liệu chính thức ít khi cảnh báo bạn](3.2-Blog2/)
Chia sẻ 4 bài học thực chiến liên quan đến các cơ chế vận hành ngầm của AWS rút ra từ trải nghiệm thực tế:
* **NAT Gateway & S3 Data Processing Fee**: Cú lừa chi phí xử lý dữ liệu khi EC2 truy cập S3 qua NAT Gateway thay vì dùng S3 Gateway Endpoint miễn phí.
* **Thảm họa chi phí khi Glacier hóa file nhỏ**: Chi phí API Transition và 32KB Metadata ẩn khi chuyển hàng triệu file nhỏ sang Glacier.
* **Lỗ hổng leo thang quyền Admin từ `iam:PassRole`**: Rủi ro nguy hiểm khi cấp quyền `iam:PassRole` đi kèm ký tự đại diện `*`.
* **Giới hạn Cooldown 6 tiếng của EBS Elastic Volume**: Quy tắc khóa chỉnh sửa dung lượng/loại ổ đĩa EBS trong 6h sau khi vừa thay đổi.

---

### [Blog 3 - Những kỹ thuật "ngầm" trên AWS: Từ tiền phạt ẩn đến những sự cố mạng vô hình](3.3-Blog3/)
Phân tích 4 vấn đề kỹ thuật chuyên sâu ở môi trường Production ít được chú ý:
* **Cross-AZ Data Transfer**: Cú sốc chi phí mạng 0.02 USD/GB khi truyền dữ liệu giữa các Availability Zone trong cùng một Region.
* **Bẫy MTU 9001 & Sự cố mạng qua VPC Peering / VPN**: Sự cố treo kết nối vô tận do Path MTU Discovery Black Hole khi Security Group chặn ICMP.
* **Trần giới hạn ngầm của DynamoDB On-Demand**: Giới hạn mở rộng tức thì tối đa gấp đôi peak traffic lịch sử trong các đợt Flash Sale.
* **Cú click chuột đắt giá trên CloudWatch Logs Insights**: Phí scan dung lượng log thô khi thực hiện câu truy vấn SQL không giới hạn thời gian.