---
title: "Worklog Tuần 2"
date: 2026-06-08
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

- **Ngày bắt đầu:** 08/06/2026
- **Ngày hoàn thành:** 13/06/2026

### Mục tiêu tuần 2:
- Nghiên cứu lý thuyết dịch vụ máy chủ ảo Amazon EC2 (Instance Types, AMIs, Key Pairs, User Data Scripts)
- Tìm hiểu các loại lưu trữ khối Amazon EBS Volumes (gp3, io3) & lưu trữ đối tượng Amazon S3
- Thực hành dựng Web Server Linux trên EC2, cấu hình EBS Storage và mã hóa dữ liệu bảo mật bằng AWS KMS

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc |
| --- | --- |
| 2 | Tìm hiểu lý thuyết Amazon EC2: Các họ máy chủ (General Purpose, Compute/Memory Optimized), mô hình tính phí (On-Demand, Reserved, Spot Instances) và cơ chế khởi tạo với User Data. |
| 3 | Nghiên cứu công nghệ lưu trữ khối Amazon EBS (Volume Types gp3/io2, Snapshots) và lưu trữ đối tượng Amazon S3 (Bucket Policies, CORS, Versioning, Storage Classes). |
| 4 | Thực hành bài Lab AWS (Amazon EC2 Compute Setup): Khởi tạo máy chủ EC2 Amazon Linux 2023, tạo Key Pair SSH, cấu hình UserData script tự động cài đặt Nginx Web Server. |
| 5 | Thực hành bài Lab AWS (EBS & S3 Storage Setup): Khởi tạo EBS Volume gp3, attach vào máy chủ EC2, format ổ đĩa ext4 và tạo S3 Bucket lưu trữ tài liệu tĩnh. |
| 6 | Tìm hiểu dịch vụ mã hóa AWS KMS: Khởi tạo Customer Managed Key (CMK), kích hoạt mã hóa Server-Side Encryption (SSE-KMS) cho S3/EBS và hoàn thiện báo cáo tuần 2. |

### Kết quả đạt được tuần 2:
* Hoàn thành đúng tiến độ công việc tuần 2 (EC2, EBS, S3 & KMS Encryption Labs).
* Nắm vững nguyên lý hoạt động của EC2, S3 và thành thạo kỹ năng mã hóa bảo mật dữ liệu với AWS KMS.

### Nguồn tài liệu hướng dẫn tham khảo:
- [Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)
- [Amazon EBS Volume Types & Performance Guide](https://docs.aws.amazon.com/ebs/)
- [Amazon S3 User Guide & Security Best Practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/)
- [AWS Key Management Service (KMS) Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)
