---
title: "Event 3"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# Summary Report: “FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!”

### Event Objectives

- **Real-World Hackathon Insights:** Summarize practical experiences, technical hurdles, and key takeaways gained during the 24-hour continuous design, development, and pitching of Agentic AI solutions on AWS Cloud infrastructure.
- **Agentic AI Systems Design (Multi-Agent Architectures):** Explore real-world production architectures of winning solutions: from Conversational Ordering Agents, Competitive Corporate Intelligence Systems, Cloud Architecture Assistants to Real-Time Crowd Dispatchers and Multi-Agent Anti-Money Laundering (AML) Investigation Engines.
- **Bridging Technology & Business Value:** Emphasize the necessity of understanding business pain points, maintaining cost efficiency, minimizing model hallucinations, and enforcing Human-in-the-Loop governance.
- **Fostering Teamwork & Lifelong Learning:** Inspire technology students to embrace real-world hackathon challenges, build professional networks, and apply cutting-edge Cloud & AI services to real problems.

---

### Keynote Speakers & Demo Teams

- **Mr. Joseph Marazota** – *Head of Technology, Asian Region* (Keynote Opening & Strategic Vision).
- **Mr. Nguyen Gia Hung** – *Head of Solution Architect, AWS Vietnam* (Founder of the **First Cloud AI Journey** program).
- **Team One Team (1st Place AWS Track):** Solution: *KFC Voice/Conversational Agent* – Multi-channel AI voice/conversational food ordering agent on Zalo/WhatsApp without requiring app downloads.
- **Team Signal Scout / Hanh Signal C (2nd Place AWS Track - FPT Students):** Solution: *Multi-Agent Corporate Intelligence* – Automated competitor signal collection, strategic analysis, and business impact forecasting.
- **Team Plan:** Solution: *SA Professional AI Native Assistant* – Automated business document analysis, architecture diagram generation (Draw.io), pricing calculator, and Terraform/CloudFormation IaC code generator.
- **Team 3K (FPT Students):** Solution: *Shepherd (Small Human Fluctuation Federation Response)* – Real-time crowd tracking and autonomous dispatcher combining YOLOv8/Fargate with AWS Kinesis Video Streams & Bedrock.
- **Team Six Pillars:** Solution: *Adaptive Workflow Engine for Anti-Money Laundering (AML)* – Multi-Agent detection, investigation, and compliance engine for banking and financial trading platforms.

---

### Event Highlights

#### 1. Keynote Address & Innovation Mindset in the AI Era (Mr. Joseph Marazota)

- **Mental Model Shift:** The Agentic AI era accelerates software release cycles from quarterly timelines (decades ago) to minutes via continuous automation.
- **Challenges & Opportunities for Young Engineers:** Encouraged young engineers to challenge legacy assumptions every single day. AI and Robotics (Amazon operates over 1 million warehouse robots) are infrastructure tools; true creative value resides in data quality, architectural design, and human-in-the-loop decisions.
- **Lifelong Learning Culture:** Continuously learn and apply new knowledge immediately to achieve sustainable career growth.

#### 2. Solution: "AI-Powered Conversational Ordering Agent" (Team One Team - AWS Track Champions)

- **Real-World Problem:** Customers encounter significant friction downloading new apps, registering accounts, and navigating complex menus to order food. McDonald's case studies reveal conversational AI is prone to hallucinations without explicit order verification.
- **Architecture & Solution:** Built a multi-channel conversational agent on Zalo/WhatsApp. Orders are handled directly inside chat threads by orchestrating an **Agentic Core** (on Amazon Bedrock) with scraping tools (TinyFish/Scraper).
- **Cost & Latency Optimization:**
  - Implemented *Agent Memory* to persist customer context and order preferences.
  - Achieved an operational cost of **~$0.006/order** (~$88/month infrastructure cost for 500 orders/day), with lightning-fast end-to-end response times of 3–5 seconds by stripping out unnecessary middleware layers.

#### 3. Solution: "Multi-Agent Corporate Intelligence" (Team Signal Scout - AWS Track Runners-Up)

- **Business Problem:** Large enterprise corporations waste massive time and manual effort tracking competitor signals across financial reports, shareholder filings, and news outlets.
- **Multi-Agent Architecture & Value Canvas:**
  - *Crawler Subagent:* Automatically ingests data using Apify (static sites) or TinyFish (dynamic/login-walled sites), pre-filtering raw content via native code to minimize Token costs and block Prompt Injection.
  - *Analysis Subagent:* Employs Bedrock Guardrails for input/output control, passing payloads to LangFuse for quality scoring; high-scoring outputs are saved to Amazon S3 & DynamoDB metadata, while low-scoring outputs trigger retry loops (max 2 attempts) before tagging for human review.
- **Cost & Compliance Takeaways:** Proposed migrating third-party tools (Apify/TinyFish/LangFuse) to AWS Native primitives (AWS Web/Browser Tools, Bedrock) to guarantee enterprise Data Residency and reduce monthly costs from $130/month to an optimal minimum.

#### 4. Solution: "SA Professional AI Native Assistant" (Team Plan)

- **Industry Pain Point:** Solution Architects (SAs) spend days manually transforming business policies and customer requirements into architecture diagrams, pricing calculators, and Infrastructure-as-Code (IaC).
- **Core Architecture & Capabilities:**
  - Analyzes natural language requirements or enterprise business documents.
  - Automatically generates standardized architecture diagrams on Draw.io using official AWS icons, allowing direct SA editing.
  - Calculates service pricing and generates deployment code (Terraform/CloudFormation) adhering to internal design best practices.
- **High Engineering Mindset:** Focused on Context & Memory management alongside error-checking mechanisms (*Validation/Blacklist Filters*) to block prohibited services and reduce code generation drift.

#### 5. Crowd Visualization "Shepherd System" (Team 3K) & AML Engine (Team Six Pillars)

- **Team 3K - Camera AI & Crowd Dispatcher:**
  - Streams video feeds via **Amazon Kinesis Video Streams**, executing object detection (YOLOv8/ByteTrack) on AWS Fargate.
  - Integrates an **Amazon Bedrock Agent** with historical memory to autonomously detect airport/retail zone congestion, firing real-time alerts and staff dispatch recommendations (*Autonomous Monitor & Operator Copilot*).
- **Team Six Pillars - Multi-Tier Anti-Money Laundering (AML):**
  - Solves the 90–95% False Positive alert problem in banking/fintech that costs $20–$25 per manual case review.
  - **3-Layer Architecture:**
    1. *Layer 1 (Fast Detection):* Kinesis Data Stream + XGBoost Model for low-cost transaction classification.
    2. *Layer 2 (Multi-Agent Investigation):* AWS Step Functions orchestrating an Orchestrator Agent and 3 Sub-agents (KYC Check, Money Flow Check, Sanction Check) combined with Amazon OpenSearch Vector RAG to compile an Evidence File.
    3. *Layer 3 (Case Management & Enterprise Trust):* Routes data through dual LLM Judges (cross-evaluation anti-hallucination) and Bedrock Guardrails, logging Reasoning Traces for final human approval on a React/Amplify dashboard.

---

### Key Takeaways

#### Agentic Engineering Mindset

- **Multi-Agent Decomposition:** Avoid using a single LLM to tackle complex problems; decompose tasks across specialized Sub-agents (Crawling, Analysis, Profiling, Decision) under an Orchestrator Agent.
- **Mitigating Hallucinations:** Combine dual-evaluation layers (LLM-as-a-Judge), rule-based Guardrails, and mandatory Reasoning Log traces for auditability.
- **Human-in-the-Loop Governance:** In high-stakes domains (Finance, Banking, Security), AI Agents act as accelerators reducing processing time from days to minutes, while humans retain final authorization (*Escalate/Approve*).

#### Cloud Engineering & Cost Optimization

- **Token & Latency Optimization:** Pre-filter raw data using native code ETL before feeding it into LLMs to conserve Tokens and mitigate Prompt Injection vectors.
- **Preference for AWS Native Services:** Maximize AWS native ecosystems (Bedrock, Step Functions, DynamoDB, Lambda, Kinesis, Amplify) over third-party utilities to ensure scalability, lower costs, and strict compliance/data residency.

#### Hackathon Execution & Career Growth

- **Scope Management:** Focus on building a Minimum Viable Product (MVP/POC) that solves a clear, specific pain point rather than over-scoping features and breaking timelines.
- **Teamwork & Humility:** Listen to technical feedback, divide tasks by core strengths (AI/SE, Backend, Business, Pitching), and maintain calm under extreme time pressure.
- **Experience Over Output:** Participating in hackathons provides an invaluable playground to test emerging technologies, embrace failure, learn from real-world feedback, and expand professional networks.

---

### Application to Projects & Personal Development

- **Implement Agentic AI Patterns:** Apply Multi-Agent design patterns (Bedrock Agents & Step Functions) to automate complex business workflows in personal and enterprise projects.
- **Integrate RAG & Vector Databases:** Construct enterprise knowledge retrieval systems combining Amazon OpenSearch Service and Bedrock Knowledge Bases to supply accurate context and eliminate hallucinations.
- **Standardize Hackathon Workflows:** Structure preparation from ideation, task division, system architecture design, documentation, to value-focused pitching.
- **Community Networking:** Actively participate in hackathons (like Agentic AI Build Week) and engage regularly with the *AWS First Cloud AI Journey* community to learn alongside Cloud Architects, recruiters, and peers.

---

### Personal Reflections & Takeaways

Observing and learning from **“FCAJ x Agentic AI Build Week: Show Up. Build. Pitch. WIN!”** delivered extraordinary practical lessons and powerful inspiration.

#### Speaker & Team Insights
- Understood Joseph Marazota's strategic vision on AI velocity and the decisive role young engineers play in reshaping industries.
- Witnessed genuine "all-night coding" stories from the 5 competing teams: from resolving overwritten `.env` files on GitHub and overcoming video streaming lag during live demos, to the shared joy of successful deployments.

#### Technical & Business Mindset
- Realized that winning solutions depend **70% on the core business pain point solved** for enterprises at optimal costs, rather than raw technical complexity.
- Learned emotional management under pressure, transforming "crazy ideas" into working production demos.

#### Timeless Lessons Learned
- **Show Up:** Take bold steps and dive into real-world hackathons without waiting until you feel "fully qualified."
- **Build:** Focus on real value, master core fundamentals, and view technology as a human-centric tool.
- **Pitch & Win:** Present solutions with deep domain understanding, embrace expert feedback, and treasure every moment with teammates.

---

> **Summary:** The event demonstrated the power of **Agentic AI Multi-Agent Architectures** on AWS Cloud, providing invaluable engineering principles and career inspiration for building next-generation cloud-native applications.

---

### Event Photos

![Agentic AI Build Week Event Presentation](/images/4-EventParticipated/4.3-Event3/event3.jpg)

