---
title: "Blog 2: Hidden Pitfalls in AWS Rarely Warned in Official Documentation"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# HIDDEN AWS PITFALLS THAT OFFICIAL DOCUMENTATION RARELY WARNS YOU ABOUT

When starting out with AWS, we often hear about familiar concepts like EC2, S3, RDS, or textbook architectural diagrams. But only when operating real production systems and paying with real money or early morning incident calls do we realize the hidden technical traps that basic courses rarely mention.

Here are 4 battle-tested lessons regarding AWS's underlying mechanisms learned after paying "tuition fees for ignorance":

---

### 1. NAT Gateway and the S3 Data Processing Cost Trap

Many guides recommend placing EC2 in Private Subnets for security and using a NAT Gateway for outbound Internet access. This architecture is correct until your application starts reading and writing data continuously to S3 (logs, backups, media uploads).

- **The Hidden Trap:** By default, traffic from EC2 to S3 routes through the NAT Gateway. NAT Gateway charges an hourly fee PLUS a per-GB Data Processing Fee.
- **The Impact:** Massive end-of-month bills solely from NAT Gateway data processing fees, even though both your EC2 and S3 instances reside in the exact same AWS Region.
- **The Fix:** Simply enable an **S3 Gateway VPC Endpoint**. It is 100% free. Data routes over AWS's internal network without touching the NAT Gateway, boosting speed and eliminating 100% of unnecessary data processing costs.

---

### 2. Archiving Small Files to Glacier: When Cost Savings Turn Into a Cost Disaster

S3 Glacier and Glacier Deep Archive are famous for ultra-low-cost long-term storage. I once confidently created an S3 Lifecycle Rule: *"Automatically transition objects older than 30 days to Glacier Deep Archive"*. However, the bucket contained millions of tiny log files (a few KB each).

- **Hidden API Request Costs:** AWS charges for Lifecycle Transition Requests ($0.03 – $0.05 per 1,000 requests). Transitioning 10 million small files costs far more in request fees than an entire year of storage savings.
- **Metadata Overhead:** For every object uploaded to Glacier, AWS adds 32KB of management metadata. A 2KB file on Glacier is billed as a 34KB file.
- **The Fix:** Before setting Glacier Lifecycle Rules, bundle (zip/tar) small files into larger archives or restrict transition rules to objects larger than 128KB.

---

### 3. Privilege Escalation Vulnerability from `iam:PassRole`

During IAM permission setup, administrators often grant `iam:PassRole` with wildcard (`*`) to developer accounts so they can attach IAM Roles to Lambda or EC2 during deployments.

- **Dangerous Scenario:** An account with permissions to create a Lambda function and wildcard `iam:PassRole` can escalate its own privileges to Administrator level.
- **How It Works:** They create a new Lambda function, attach an existing system IAM Role containing `AdministratorAccess`, and write a small script inside Lambda to execute any action under Admin authority.
- **The Fix:** `iam:PassRole` is as powerful as Admin privileges. Always specify exact allowed Role ARNs instead of using wildcards (`*`).

---

### 4. The Hidden 6-Hour Cooldown Limit on EBS Volumes

AWS Elastic Volumes allow expanding capacity or changing volume types (e.g., gp2 to gp3) on a running EC2 instance without downtime. While convenient, there is a hidden time restriction.

- **Cooldown Period:** Once you confirm an EBS Volume modification, AWS locks further edits on that volume for exactly 6 hours (6h).
- **Real Incident:** During a disk full emergency, a engineer hastily expands a volume from 100GB to 120GB to resolve the alert. Moments later, they realize 120GB is insufficient and need 500GB. AWS rejects the request and forces a 6-hour wait.
- **The Fix:** When modifying EBS Volumes, calculate a generous capacity margin on your first edit, as you won't have another chance for the next 6 hours.

---

### Final Thoughts

AWS is vast and official documentation usually focuses on ideal use cases. In production operations, small technical details and hidden limits determine system stability and financial safety.

📌 **Link to Facebook Community Post:**  
[https://www.facebook.com/share/p/18wTHQFBVC/](https://www.facebook.com/share/p/18wTHQFBVC/)