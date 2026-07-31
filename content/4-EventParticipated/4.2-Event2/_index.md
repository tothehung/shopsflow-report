---
title: "Event 2"
date: 2026-07-11
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Summary Report: "AWS Technical Deep Dive & Certification Roadmap (July 11)"

### Event Objectives

- **AI/Agent Integration in Security:** Introduce AWS Security Agent (Frontier Agent) powered by Amazon Bedrock to automate the full web application security lifecycle (Design, Code, Pentest).
- **Standardizing Operations & Observability Mindset:** Shift operational focus from traditional infrastructure monitoring (SLA / Infrastructure metrics) to user-centric observability (User Experience & Business metrics).
- **AWS Certification Strategy:** Provide a strategic preparation roadmap for the AWS Certified Cloud Practitioner (CLF-C02) exam, including real-world test-taking techniques and interview strategies.

---

### Keynote Speakers

- **Thinh Nguyen** – *DevOps/DevSecOps/Cloud Engineer, Styl Solutions* (First Cloud AI Journey Member).
- **Nguyen Huynh Son** – *Infrastructure Support Engineer, Endava* (Ex-Infrastructure Reliability Engineer at SPS, AWS Student Builder Group HUFLIT Member).
- **Ngo Le Tan Huy** – *Presenter / Cloud Engineering Enthusiast*.

---

### Event Highlights

#### 1. Securing Your Web Apps With AWS Security Agent (Speaker: Thinh Nguyen)

- **The Traditional Security Bottleneck:**
  - Manual penetration testing is time-consuming (takes weeks), costly ($5,000 – $20,000 per assessment), and highly dependent on individual engineer skill/fatigue.
- **AWS Security Agent (Frontier Agent):**
  - **Autonomous Reasoning:** Leverages Amazon Bedrock to autonomously plan and execute security tasks without human intervention.
  - **Full Security Lifecycle Automation:**
    1. *Design Security Review:* Evaluates Markdown design docs or Terraform IaC code against Managed Packs (PCI DSS, NIST CSF, AWS Well-Architected). Free tier: 200 reviews/month.
    2. *Code Security Review:* Integrates directly into GitHub/GitLab Pull Requests, automatically commenting on vulnerabilities and proposing code fixes (Auto-PR Fixes). Free tier: 1,000 PR reviews/month.
    3. *Automated Pentesting:* Executes multi-step exploit chains (e.g., IDOR -> XSS), authenticates as a real user, and generates Verifiable Findings with proof-of-exploit reports.
- **Costs & Practical Limitations:**
  - *Pricing:* Pay-as-you-go at $50/Task-Hour (extremely cost-effective compared to traditional $10,000 pentest engagements).
  - *Limitations:* Vulnerable to complex authentication mechanisms (MFA, Biometrics, mTLS); struggles with nuanced business logic flaws.

![Securing Your Web Apps With AWS Security Agent Presentation](/images/4-EventParticipated/4.2-Event2/event2-1.jpg)

---

#### 2. SLA and Monitoring: From SLA to Monitoring What Really Matters (Speaker: Nguyen Huynh Son)

- **SLA Definition & Risk Management:**
  - SLA (Service Level Agreement) is a formal commitment defining service availability between provider and client.
  - Monitoring is a core component of risk management: *Identify risk -> Monitor signals -> Respond -> Improve*.
- **The Gap Between "Healthy Infrastructure" and "Happy Users":**
  - **The Monitoring Pyramid:**
    1. *Customer Experience:* Can the user log in and complete checkout? (Top priority)
    2. *Business Metrics:* Login success rate, order count, revenue.
    3. *Application Metrics:* Latency, Error rate, Request throughput.
    4. *Infrastructure Metrics:* CPU, Memory, Disk, Network.
    5. *Cloud Provider:* EC2, RDS, ALB, S3.
  - **Harsh Reality:** All infrastructure metrics reporting "Green" (CPU 18%, ALB Target 2/2, Healthcheck `/health` 200 OK) does not guarantee a good user experience. If database connectivity fails, `/login` returns errors while server monitoring stays green.
- **Operational Advice:** *"Everything fails all the time, so plan for failure and nothing fails"* (Dr. Werner Vogels - CTO, Amazon). Enterprises are responsible for Customer Experience, not just server uptime.

![SLA and Monitoring Presentation](/images/4-EventParticipated/4.2-Event2/event2-2.jpg)

---

#### 3. Inside The Exam: AWS Cloud Practitioner (Speaker: Ngo Le Tan Huy)

- **AWS Certified Cloud Practitioner (CLF-C02) Exam Structure:**
  - 65 multiple-choice/multiple-response questions, 90-minute duration (+30 mins for non-native English speakers via Accommodation request), passing score $\ge 700/1000$.
  - **Weight Distribution Across 4 Domains:**
    1. *Domain 1: Cloud Concepts (24%)* – Digital transformation, 6 Cloud benefits, AWS WAF, AWS CAF.
    2. *Domain 2: Security and Compliance (30%)* – Shared Responsibility Model, IAM (Least Privilege), Security Groups vs NACLs, AWS Shield/WAF, AWS Artifact.
    3. *Domain 3: Cloud Technology and Services (34%)* – Core concepts & use cases for Compute, Storage, Database, Networking.
    4. *Domain 4: Billing, Pricing, and Support (12%)* – EC2 pricing models, AWS Cost Explorer, AWS Budgets, Support Plans.
- **Effective Exam Preparation & Strategies:**
  - **Map Keyword Thinking:** Link service names directly to 1–2 core keywords (e.g., *"Decouple/Microservices"* -> SQS; *"Data monetization"* -> Business Perspective in CAF).
  - **Analyze Wrong Options:** Understand why option A is correct and B, C, D are incorrect to avoid exam traps.
  - **Test-Taking Tips:** Use the Process of Elimination to discard fake services; use *"Flag for review"* for uncertain questions; watch for key qualifiers (*NOT, Least cost, Most scalable*).

![Inside The Exam AWS Cloud Practitioner Presentation](/images/4-EventParticipated/4.2-Event2/event2-3.jpg)

---

### Key Takeaways

#### DevSecOps & AI Mindset

- **Shift to Automated Pentesting:** Harness Generative AI Agents for continuous security testing within CI/CD pipelines instead of relying solely on annual manual audits.
- **Verifiable Proof:** Evaluate security risks based on verified exploitation proofs rather than raw alerts from static analysis tools (SAST/DAST).

#### Observability Mindset

- **User-Centric Monitoring:** Construct Dashboards and Metrics around actual user flows (Login, Checkout, Payment) rather than raw CPU/Memory metrics.
- **Automated Incident Flow:** Establish end-to-end alerting: *Custom Metric -> CloudWatch Alarm -> SNS Topic -> Email/Slack* to resolve issues before users report them.

#### Cloud Certification Strategy

- **Learn via Use Cases & Keywords:** Focus on big-picture understanding and service purposes rather than memorizing syntax.
- **Hands-On Practice:** Utilize the AWS Free Tier to gain practical experience with core services, solidifying theoretical concepts.

---

### Application to Projects & Personal Development

- **Implement DevSecOps in Workloads:**
  - Integrate automated code scanning and Terraform IaC architecture checks into GitHub Actions / GitLab CI pipelines.
- **Build Comprehensive Observability:**
  - Configure Synthetic Monitoring and Custom Metrics tracking critical API success rates (login, checkout).
  - Setup CloudWatch Alarms integrated with Amazon SNS for real-time notifications via Telegram/Slack when business flows break.
- **AWS Certification Roadmap:**
  - Complete the *Cloud Practitioner Essentials* course on AWS Skill Builder.
  - Practice mock exams, apply elimination techniques, and analyze exam traps to confidently achieve CLF-C02 certification.

---

### Personal Reflections & Takeaways

Attending the July 11 technical session provided invaluable practical insights, updated cutting-edge security technologies, and standardized cloud operations thinking.

#### Speaker Insights
- **Thinh Nguyen:** Expanded my perspective on deploying AI Agents for automated pentesting on AWS, recognizing both advantages and cost/technical boundaries.
- **Nguyen Huynh Son:** Reshaped my view on Observability, reinforcing *"Healthy infrastructure $\neq$ Happy users"* and the necessity of user-experience monitoring.
- **Ngo Le Tan Huy:** Provided a complete exam strategy, keyword mapping techniques, and confidence tips to conquer the AWS Certified Cloud Practitioner exam.

#### Core Principles Learned
- **Security-First & Automation:** Security must be embedded from initial design (Design Review) and code (Code Review) via automation.
- **Responsibility for User Experience:** System health extends beyond keeping servers alive; business transactions must succeed end-to-end.
- **Strategic Preparation:** Whether operating cloud systems or preparing for certification exams, understanding core fundamentals and executing strategically determines success.

![AWS Technical Deep Dive Event Group Photo](/images/4-EventParticipated/4.2-Event2/event2-4.jpg)

---

> **Summary:** The event equipped me with both practical engineering mindsets (Security AI, Observability) and a personal career growth roadmap (AWS Certification), laying a solid foundation for my journey toward becoming a professional Cloud / DevSecOps Engineer.
