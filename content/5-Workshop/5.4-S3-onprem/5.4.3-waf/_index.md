---
title: "Protect with AWS WAF"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

In this step, you will create an **AWS WAF Web ACL** and attach it to the CloudFront distribution. WAF will inspect every HTTP/HTTPS request to the Shopsflow storefront and block common web threats before they reach your application.

---

### 1. Why WAF for Shopsflow?

The Shopsflow storefront is a public-facing React application that exposes an API backend. Without WAF:

- Attackers can probe the API for **SQL Injection** vulnerabilities in search and filter parameters.
- Bots can **enumerate product IDs** or perform **credential stuffing** attacks on the login endpoint.
- Malicious inputs can **bypass input validation** in the frontend.

AWS WAF with Managed Rule Groups provides instant protection against these threats without writing custom rules.

---

### 2. Create WAF Web ACL

{{% notice warning %}}
AWS WAF for CloudFront must be created in the **US East (N. Virginia) `us-east-1`** region, regardless of where your other resources are deployed.
{{% /notice %}}

1. Navigate to **AWS Console** → Switch region to **US East (N. Virginia)**.
2. Navigate to **WAF & Shield** → **Web ACLs** → **Create web ACL**.

#### General Settings

| Field         | Value                           |
| ------------- | ------------------------------- |
| Name          | `shopsflow-waf`                 |
| Resource type | Amazon CloudFront distributions |
| Region        | Global (CloudFront)             |

---

### 3. Add Managed Rule Groups

Click **Add rules** → **Add managed rule groups** → Enable the following three rule groups:

#### Rule Group 1: Common Web Exploits

- **Name:** `AWSManagedRulesCommonRuleSet`
- **Purpose:** Blocks common web application vulnerabilities (XSS, path traversal, HTTP flooding)
- **Action:** Block

#### Rule Group 2: Known Bad Inputs

- **Name:** `AWSManagedRulesKnownBadInputsRuleSet`
- **Purpose:** Blocks requests that match patterns known to be exploits, such as Log4SHELL
- **Action:** Block

#### Rule Group 3: SQL Injection Protection

- **Name:** `AWSManagedRulesSQLiRuleSet`
- **Purpose:** Blocks SQL injection attacks in query strings, body, headers, and URI paths
- **Action:** Block

Click **Add rules**.

---

### 4. Set Default Action and Priority

1. Under **Set rule priority**: Leave the default order (rules are evaluated top-to-bottom).
2. Under **Default action**: **Allow** — WAF will allow all traffic not matched by the above rules.

---

### 5. Configure CloudWatch Metrics

Under **Configure metrics**:

- Web ACL metric name: `shopsflow-waf-metrics`
- Enable sampled requests — allows you to see sample requests in the WAF console for debugging.

Click **Next** → **Next** → **Create web ACL**.

---

### 6. Associate WAF with CloudFront

1. Navigate to **WAF** → **Web ACLs** → Select `shopsflow-waf`.
2. Click the **Associated AWS resources** tab → **Add AWS resources**.
3. Select **Amazon CloudFront distribution** → Choose the `shopsflow` distribution.
4. Click **Add**.

{{% notice note %}}
It takes **1–2 minutes** for the WAF association to propagate globally across all CloudFront edge locations.
{{% /notice %}}

---

### 7. Test WAF Protection

#### Test 1: SQL Injection Block

```bash
# This request simulates a SQL injection in a query parameter
curl -I "https://dxxxxx.cloudfront.net/api/products?search=1'+OR+'1'='1"
# Expected: HTTP/2 403 (blocked by WAF - SQLi rule)
```

#### Test 2: XSS Block

```bash
# This simulates a cross-site scripting payload
curl -I "https://dxxxxx.cloudfront.net/?q=<script>alert(1)</script>"
# Expected: HTTP/2 403 (blocked by WAF - CommonRuleSet)
```

#### Test 3: Normal Request Passes

```bash
# A normal product browse request should succeed
curl -I "https://dxxxxx.cloudfront.net/api/products?page=1&size=10"
# Expected: HTTP/2 200 OK
```

---

### 8. View WAF Metrics in CloudWatch

1. Navigate to **CloudWatch** → **Metrics** → **WAF** → `shopsflow-waf-metrics`.
2. Monitor:
   - `BlockedRequests` — number of requests blocked per rule
   - `AllowedRequests` — total legitimate traffic
   - `CountedRequests` — requests matched by count-only rules

**The Shopsflow frontend is now protected by AWS WAF. All SQL injection, XSS, and known bad input patterns are automatically blocked at the CloudFront edge before reaching the ALB and backend.**
