# 🚨 AI Security – SIEM Detection Rules Playbook

This document contains **SOC-ready detection logic** for identifying **AI-related security threats** using major SIEM platforms:

- Splunk  
- Microsoft Sentinel (KQL)  
- IBM QRadar  

These rules are **use-case driven**, focused on **analyst thinking**, and suitable for:
- SOC operations
- Interview discussions
- Detection engineering practice

> ⚠️ Note: These rules are **logical examples**, not production-hardened signatures.  
> They must be tuned for environment, noise, and false positives.

---

## 1️⃣ Prompt Injection Attacks (Outsider Threat)

### 🎯 Goal
Detect attempts to override AI system instructions using malicious prompts.

### 🔹 Splunk
```spl
index=ai_logs
("ignore previous instructions" OR "system prompt" OR "act as admin" OR "bypass safety")
| stats count by src_ip, user
| where count > 5
```

### 🔹 Microsoft Sentinel (KQL)
```kql
AILogs
| where Prompt has_any ("ignore previous", "system prompt", "admin mode", "bypass safety")
| summarize count() by IPAddress, User
| where count_ > 5
```

### 🔹 IBM QRadar (Logic)
```text
WHEN AI Application Log CONTAINS override keywords
AND SAME SOURCE IP ATTEMPTS > 5 TIMES IN 10 MINUTES
```

---

## 2️⃣ AI-Generated Phishing & Deepfake Abuse (Hybrid Threat)

### 🔹 Splunk
```spl
index=auth_logs action="high_risk"
| join user [ search index=email_logs anomaly_score>80 ]
```

### 🔹 Microsoft Sentinel (KQL)
```kql
SigninLogs
| where RiskLevelDuringSignIn == "high"
| join EmailEvents on UserPrincipalName
```

### 🔹 IBM QRadar
```text
WHEN High-Risk Authentication
FOLLOWED BY Financial OR Privileged ACTION
WITHIN 15 MINUTES
```

---

## 3️⃣ Shadow AI Usage (Insider Threat)

### 🔹 Splunk
```spl
index=proxy_logs
| where url IN ("chat.openai.com", "claude.ai", "bard.google.com")
| where bytes_out > 50000
```

### 🔹 Microsoft Sentinel (KQL)
```kql
CommonSecurityLog
| where DestinationHostName contains "openai"
| where SentBytes > 50000
```

### 🔹 IBM QRadar
```text
WHEN Large File Upload
TO Unapproved AI Domain
```

---

## 4️⃣ Training Data Poisoning (Insider or Outsider)

### 🔹 Splunk
```spl
index=ml_training_logs
| stats avg(model_accuracy) by model_name
| where model_accuracy < threshold
```

### 🔹 Microsoft Sentinel (KQL)
```kql
MLTrainingLogs
| summarize avg(Accuracy) by ModelName
| where avg_Accuracy < ExpectedBaseline
```

### 🔹 IBM QRadar
```text
WHEN Model Accuracy DROPS SIGNIFICANTLY
AFTER New Training Data Ingestion
```

---

## 5️⃣ Model Inversion / Data Extraction Attempts (Outsider Threat)

### 🔹 Splunk
```spl
index=ai_api_logs
| stats count by api_key, query_pattern
| where count > 100
```

### 🔹 Microsoft Sentinel (KQL)
```kql
AIAPILogs
| summarize count() by APIKey, QueryPattern
| where count_ > 100
```

### 🔹 IBM QRadar
```text
WHEN Excessive Structured Queries
FROM SAME API KEY
```

---

## 6️⃣ Over-Privileged AI Agents (Insider / Misconfiguration)

### 🔹 Splunk
```spl
index=cloud_audit
| where user_type="ai_service_account"
| where privilege="admin"
```

### 🔹 Microsoft Sentinel (KQL)
```kql
AzureActivity
| where Caller contains "ai"
| where ActivityStatus == "Succeeded"
| where OperationName contains "Delete" or OperationName contains "Update"
```

### 🔹 IBM QRadar
```text
WHEN AI Service Account
PERFORMS ADMIN ACTION
OUTSIDE BUSINESS HOURS
```

---

## 7️⃣ AI Hallucination-Driven Configuration Errors (Indirect Insider Risk)

### 🔹 Splunk
```spl
index=change_logs
| where change_type="security"
| where approval_status="none"
```

### 🔹 Microsoft Sentinel (KQL)
```kql
ConfigurationChange
| where Approved == false
| where ChangeType == "Security"
```

### 🔹 IBM QRadar
```text
WHEN Security Configuration CHANGED
WITHOUT APPROVAL
```

---

## 🧾 SOC Analyst Notes

- Treat AI tools like **high-risk service accounts**
- Combine SIEM with **UEBA, DLP, IAM**
- Always tune thresholds to reduce false positives
- Maintain **human-in-the-loop** for AI-driven actions

---


## 📌 Disclaimer

This content is for **educational and portfolio purposes**.  
Production deployments require tuning, validation, and organizational approval.
