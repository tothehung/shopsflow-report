---
title: "Worklog Week 7"
date: 2026-07-13
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

- **Start Date:** July 13, 2026
- **Completion Date:** July 18, 2026

### Objectives for Week 7:
- Theoretical research on Online Payment Gateway integration (VNPay Gateway) & Cloudinary media management APIs
- Hands-on AWS Lab: Build Event-Driven notification messaging with Amazon SQS Queues and SNS Topics
- Complete VNPay Sandbox payment, Cloudinary product image uploads, order status workflow (`PENDING → PAID → SHIPPED → DELIVERED`) & Admin Reviews Moderation for Shopsflow

### Tasks to implement this week:
| Day | Task |
| --- | --- |
| 2 | Study VNPay Payment Gateway integration mechanics: Sandbox URL generation, HMAC-SHA512 checksum hashing, IPN Callback processing, and return codes (vnp_ResponseCode). |
| 3 | Research Cloudinary SDK media storage for Spring Boot/React applications and event messaging architectures using Amazon SQS & SNS. |
| 4 | Hands-on AWS Lab (Event-Driven Messaging Setup): Provision SQS Standard Queue, SNS Topic, and configure notification access policies for payment events. |
| 5 | Develop VNPay payment module on Spring Boot backend, integrate Cloudinary API for product image uploads, and build Admin Reviews Moderation UI on React Frontend. |
| 6 | Test E2E checkout & VNPay payment flow, auto-update order statuses from `PENDING` to `PAID` / `SHIPPED`, and compile Week 7 progress report. |

### Results achieved in Week 7:
* Completed Week 7 tasks on schedule (VNPay Integration, Cloudinary & Admin Order Workflow).
* Successfully integrated VNPay payment gateway, automated order processing workflows, and built Admin review moderation features for Shopsflow.

### References & Study Materials:
- [VNPay Payment Gateway Integration Specs](https://sandbox.vnpayment.vn/apis/vnpay-payment/)
- [Cloudinary Media Upload Java SDK Documentation](https://cloudinary.com/documentation/java_integration)
- [Amazon SQS & SNS Messaging Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/)
