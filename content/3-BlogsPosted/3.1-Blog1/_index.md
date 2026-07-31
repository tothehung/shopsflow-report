---
title: "Blog 1: 3 Hidden AWS Technical Gotchas That Cause Production Incidents"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# 3 HIDDEN AWS TECHNICAL GOTCHAS THAT NOBODY TELLS YOU, BUT WILL CAUSE PRODUCTION INCIDENTS

Instead of discussing high-level architectural concepts, this article summarizes 3 subtle AWS technical details. They rarely appear on presentation slides, yet frequently cause unexpected costs or require days of debugging.

---

### 1. "Invisible" Files on S3: Incomplete Multipart Uploads Charging Monthly Fees

When an application uploads a large file to Amazon S3 and gets interrupted due to a network drop, the upload process aborts.

- **The Hidden Truth:** Data fragments uploaded prior to the disconnection remain stored on AWS S3 infrastructure, and AWS quietly charges you monthly storage fees for these incomplete uploads.
- **The Catch:** You cannot view these incomplete parts on the standard AWS S3 Console or via regular `aws s3 ls` CLI commands. If thousands of large file uploads fail, these hidden storage fees can accumulate significantly.
- **The Fix:** Always enable an S3 Lifecycle Rule and check **Delete incomplete multipart uploads** after 1–2 days to automatically purge hidden incomplete uploads.

---

### 2. The IMDSv2 Hop Limit Trap in Containerized Environments

Upgrading from IMDSv1 to IMDSv2 is a security requirement to prevent EC2 IAM Role credential leaks. However, if your application runs inside a Docker container on that EC2 instance, it immediately loses access to the AWS SDK.

- **The Root Cause:** IMDSv2 uses IP Packet TTL (Time to Live) to prevent Session Hijacking. By default, AWS sets `PutResponseHopLimit = 1`.
- **The Incident:** Requests originating inside a Docker Container hop across Docker's virtual bridge network (`veth`) before reaching the metadata IP (`169.254.169.254`), resulting in 2 network hops. The packet is dropped instantly because it exceeds the `HopLimit = 1` threshold.
- **The Fix:** Increase the `Metadata Response Hop Limit` setting of the EC2 instance from 1 to 2.

---

### 3. AWS Lambda's /tmp Directory Isn't As Clean As You Think

Many developers assume that every Lambda function invocation executes inside a completely isolated, clean environment.

- **The Hidden Truth:** Due to Warm Start mechanisms, AWS reuses existing containers for subsequent invocations to optimize cold start performance. Consequently, temporary files written to the `/tmp` directory in previous requests persist for future invocations.
- **The Impact:**
  - **Security Risk:** If temporary files containing sensitive User A data are not deleted, User B's subsequent request (sharing the same warm container) might read that data.
  - **Disk Exhaustion:** The `/tmp` directory gradually fills up over multiple invocations, leading to intermittent `No space left on device` errors that are hard to trace.
- **The Fix:** Always wrap temporary file operations inside a `try...finally` block to proactively delete files immediately after processing, regardless of Lambda's container lifecycle.

---

> Hopefully these 3 tips help your AWS infrastructure run smoother and avoid unexpected production issues.

📌 **Link to Facebook Community Post:**  
[https://www.facebook.com/groups/awsstudygroupfcj/permalink/2229330564498570/](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2229330564498570/?rdid=zDzoFSO3Oeba6zuF#)