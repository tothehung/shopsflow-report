---
title: "Blogs Posted"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

This section summarizes technical articles and hands-on AWS cloud experience posts published on the [AWS Study Group FCJ Community](https://www.facebook.com/groups/awsstudygroupfcj).

---

### [Blog 1 - 3 Hidden AWS Technical Gotchas That Cause Production Incidents](3.1-Blog1/)
A breakdown of 3 technical details on AWS rarely covered in certification courses that cause unexpected charges or days of debugging:
* **S3 Incomplete Multipart Uploads**: Incomplete upload data fragments quietly incurring monthly storage fees while remaining invisible on the S3 Console.
* **IMDSv2 Hop Limit Trap**: Loss of AWS SDK credentials inside Docker containers due to default Hop Limit = 1.
* **AWS Lambda `/tmp` Directory**: Temporary file persistence across invocations due to container Warm Starts.

---

### [Blog 2 - Hidden AWS Pitfalls That Official Documentation Rarely Warns You About](3.2-Blog2/)
4 battle-tested lessons regarding AWS underlying operational behaviors learned from production experience:
* **NAT Gateway & S3 Data Processing Costs**: Unexpected processing fees when EC2 routes S3 traffic through NAT Gateways instead of free S3 Gateway VPC Endpoints.
* **Glacier Archiving Small File Cost Disaster**: API Transition request fees and 32KB metadata overhead when archiving millions of tiny files to S3 Glacier.
* **Privilege Escalation via `iam:PassRole`**: Security risks of granting wildcard `iam:PassRole` permissions to non-admin roles.
* **EBS Elastic Volume 6-Hour Cooldown Limit**: The 6-hour modification lock enforced by AWS after editing an EBS Volume.

---

### [Blog 3 - Hidden AWS Technical Gotchas: From Hidden Costs to Invisible Network Outages](3.3-Blog3/)
Analysis of 4 deep technical challenges encountered in production environments:
* **Cross-AZ Data Transfer**: The $0.02/GB bandwidth cost shock when routing inter-service traffic across Availability Zones within the same Region.
* **MTU 9001 Trap & Connection Drops via VPC Peering / VPN**: Path MTU Discovery Black Hole incidents caused by blocking ICMP messages in Security Groups.
* **DynamoDB On-Demand Hidden Scaling Limits**: Instant scaling capped at double historical peak traffic during sudden Flash Sales.
* **Expensive Clicks on CloudWatch Logs Insights**: Raw log scanning fees incurred when running unconstrained SQL queries across large log groups.