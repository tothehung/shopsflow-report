---
title: "Worklog Week 2"
date: 2026-06-08
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

- **Start Date:** June 08, 2026
- **Completion Date:** June 13, 2026

### Objectives for Week 2:
- Theoretical research on Amazon EC2 virtual servers (Instance Types, AMIs, Key Pairs, User Data Scripts)
- Study Amazon EBS Block Storage (gp3, io3) and Amazon S3 Object Storage architecture
- Hands-on Linux Web Server deployment on EC2, EBS Volume attach, and AWS KMS data encryption configuration

### Tasks to implement this week:
| Day | Task |
| --- | --- |
| 2 | Study Amazon EC2 theory: Instance Families (General Purpose, Compute/Memory Optimized), Purchasing Options (On-Demand, Reserved, Spot Instances), and User Data automation. |
| 3 | Research Amazon EBS block storage (gp3/io2 volume types, Snapshots) and Amazon S3 object storage (Bucket Policies, CORS, Versioning, Storage Classes). |
| 4 | Hands-on AWS Lab (Amazon EC2 Compute Setup): Provision Amazon Linux 2023 EC2 instance, generate SSH Key Pair, and configure UserData script to auto-install Nginx Web Server. |
| 5 | Hands-on AWS Lab (EBS & S3 Storage Setup): Create gp3 EBS Volume, attach to EC2 instance, format ext4 filesystem, and create S3 Bucket for static asset hosting. |
| 6 | Research AWS KMS service: Create Customer Managed Key (CMK), enable Server-Side Encryption (SSE-KMS) for S3 buckets & EBS volumes, and compile Week 2 report. |

### Results achieved in Week 2:
* Completed Week 2 tasks on schedule (EC2, EBS, S3 & KMS Encryption Labs).
* Mastered core principles of EC2 compute, S3 storage, and data encryption techniques using AWS KMS.

### References & Study Materials:
- [Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)
- [Amazon EBS Volume Types & Performance Guide](https://docs.aws.amazon.com/ebs/)
- [Amazon S3 User Guide & Security Best Practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/)
- [AWS Key Management Service (KMS) Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)
