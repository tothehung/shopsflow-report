---
title: "Worklog Tuần 7"
date: 2026-07-13
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

- **Ngày bắt đầu:** 13/07/2026
- **Ngày hoàn thành:** 18/07/2026

### Mục tiêu tuần 7:
- Nghiên cứu lý thuyết Cổng thanh toán trực tuyến (VNPay Payment Gateway) & Dịch vụ quản lý media Cloudinary API
- Thực hành bài Lab AWS: Xây dựng luồng thông báo sự kiện với Amazon SQS Queue và SNS Notification Topics
- Tích hợp thanh toán VNPay Sandbox, tải ảnh sản phẩm Cloudinary và quy trình chuyển trạng thái đơn hàng (`PENDING → PAID → SHIPPED → DELIVERED`) & Admin Reviews Moderation cho Shopsflow

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc |
| --- | --- |
| 2 | Nghiên cứu quy trình tích hợp VNPay Payment Gateway: Tạo URL thanh toán Sandbox, mã hóa checksum HMAC-SHA512, xử lý IPN Callback URL và mã trả về (vnp_ResponseCode). |
| 3 | Tìm hiểu dịch vụ lưu trữ media đám mây Cloudinary SDK cho Spring Boot/React và kiến trúc hàng đợi nhắn tin sự kiện Amazon SQS & SNS. |
| 4 | Thực hành bài Lab AWS (Event-Driven Messaging Setup): Khởi tạo SQS Standard Queue, SNS Topic và cấu hình chính sách phân phối thông báo sự kiện thanh toán. |
| 5 | Lập trình module thanh toán VNPay trên Spring Boot backend Shopsflow, tích hợp Cloudinary API upload ảnh sản phẩm và giao diện duyệt đánh giá Admin Reviews Moderation trên React Frontend. |
| 6 | Kiểm thử E2E luồng đặt hàng và thanh toán VNPay, tự động cập nhật trạng thái đơn hàng từ `PENDING` sang `PAID` / `SHIPPED` và hoàn thiện báo cáo tuần 7. |

### Kết quả đạt được tuần 7:
* Hoàn thành đúng tiến độ công việc tuần 7 (VNPay Integration, Cloudinary & Admin Order Workflow).
* Tích hợp thành công cổng thanh toán VNPay, tự động hóa luồng xử lý đơn hàng và quản lý đánh giá sản phẩm chuyên nghiệp cho quản trị viên Shopsflow.

### Nguồn tài liệu hướng dẫn tham khảo:
- [VNPay Payment Gateway Integration Specs](https://sandbox.vnpayment.vn/apis/vnpay-payment/)
- [Cloudinary Media Upload Java SDK Documentation](https://cloudinary.com/documentation/java_integration)
- [Amazon SQS & SNS Messaging Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/)
