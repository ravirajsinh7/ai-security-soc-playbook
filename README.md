# 🤖 AI Security Risks & SOC Detection Playbook

A **practical, SOC-focused guide** for Cyber Security Analysts to understand **AI-related security risks**, how they appear in the real world, and how they can be **detected and mitigated using existing SOC tools**.

This repository focuses on **detection and response**, not theory.

---

## 📌 Why This Repository Exists

AI systems introduce **new attack surfaces**, but attackers still rely on **classic techniques** such as:
- Social engineering  
- Privilege abuse  
- Data exfiltration  
- Misconfigurations  

AI **amplifies** these attacks by making them:
- Faster  
- More convincing  
- Harder to detect  

This playbook bridges the gap between **AI security risks** and **SOC-level monitoring & response**.

---

## 🧠 AI Threat Categories

AI-related risks fall into **three main categories**:

| Category | Description |
|--------|------------|
| **Outsider Threats** | External attackers abusing AI interfaces |
| **Insider Threats** | Authorized users misusing AI systems |
| **Hybrid Threats** | Outsiders using AI to impersonate trusted insiders |

---

## 🚨 AI Threats & SOC Detection Use-Cases

---

### 1️⃣ Prompt Injection Attacks (Outsider Threat)

**Risk**  
Attackers manipulate AI prompts to bypass safeguards, override instructions, or extract sensitive information.

**SOC Data Sources**
- AI application logs  
- API gateway logs  
- Web Application Firewall (WAF) logs  

**Detection Indicators**
- Repeated prompt override attempts (e.g., “ignore previous instructions”)  
- High-frequency requests from a single IP or session  
- Abnormally long or sensitive responses  

**SOC Action**
- Block or throttle the source  
- Review prompt and response logs  
- Tune input validation and WAF rules  

---

### 2️⃣ AI-Generated Phishing & Deepfakes (Hybrid Threat)

**Risk**  
AI enables highly realistic phishing emails, voice calls, or video impersonation that manipulate user trust.

**SOC Data Sources**
- Email security logs  
- UEBA (User & Entity Behavior Analytics)  
- Authentication and IAM logs  

**Detection Indicators**
- High-risk action following unusual communication  
- Behavioral anomalies (new device, location, or timing)  
- MFA fatigue or repeated authentication attempts  

**SOC Action**
- Pause or reverse sensitive actions  
- Perform out-of-band verification  
- Reset credentials if compromise is suspected  

---

### 3️⃣ Shadow AI Usage (Insider Threat)

**Risk**  
Employees upload sensitive data into unapproved public AI tools.

**SOC Data Sources**
- Data Loss Prevention (DLP)  
- Proxy / Secure Web Gateway  
- CASB  

**Detection Indicators**
- Upload of source code, credentials, or PII  
- Access to unapproved AI platforms  
- Large copy-paste activity from internal systems  

**SOC Action**
- Notify data owners  
- Educate the user  
- Enforce AI usage policies  

---

### 4️⃣ Training Data Poisoning (Insider or Outsider)

**Risk**  
Malicious or manipulated data alters AI model behavior and decisions.

**SOC Data Sources**
- ML training pipelines  
- Data validation logs  
- Model performance metrics  

**Detection Indicators**
- Sudden accuracy drops or model drift  
- Increase in false negatives or false positives  
- Unauthorized or unusual data sources  

**SOC Action**
- Roll back affected datasets or models  
- Validate data integrity  
- Escalate to ML and security teams  

---

### 5️⃣ Model Inversion & Data Leakage (Outsider Threat)

**Risk**  
Attackers extract sensitive training data through repeated and structured AI queries.

**SOC Data Sources**
- AI API logs  
- Application telemetry  

**Detection Indicators**
- Repetitive, patterned queries  
- Attempts to reconstruct personal or sensitive data  
- Rate-limit violations  

**SOC Action**
- Enforce strict rate limiting  
- Mask or restrict outputs  
- Review training data exposure  

---

### 6️⃣ Over-Privileged AI Agents (Insider / Misconfiguration)

**Risk**  
AI service accounts or automation tools have excessive permissions.

**SOC Data Sources**
- Cloud IAM logs  
- Audit logs  
- SOAR execution logs  

**Detection Indicators**
- AI accounts performing admin-level actions  
- Resource deletion or modification  
- Activity outside expected time windows  

**SOC Action**
- Disable or restrict AI service accounts  
- Apply least privilege principles  
- Review automation workflows  

---

### 7️⃣ AI Hallucination-Driven Errors (Indirect Insider Risk)

**Risk**  
Incorrect AI-generated guidance leads to insecure configurations or operational mistakes.

**SOC Data Sources**
- Change management systems  
- Cloud configuration monitoring  
- Firewall and security logs  

**Detection Indicators**
- Unauthorized configuration changes  
- Rapid rollbacks after deployment  
- Configuration drift from baselines  

**SOC Action**
- Roll back changes  
- Enforce approval workflows  
- Maintain human-in-the-loop validation  

---

## 🧾 SOC Quick Reference Table

| AI Threat | Primary SOC Tool |
|---------|----------------|
| Prompt Injection | WAF, API Logs |
| AI Phishing & Deepfakes | UEBA, Email Security |
| Shadow AI | DLP, CASB |
| Training Data Poisoning | ML Monitoring |
| Model Inversion | API Monitoring |
| Over-Privileged AI | IAM, Cloud Logs |
| Hallucination Errors | Config Monitoring |

---

## 🎯 Key Takeaways

- AI does **not replace traditional attacks** — it amplifies them  
- Existing SOC tools can detect AI threats with **updated detection logic**  
- Identity, behavior, and permissions are critical control points  
- **Human oversight remains essential**  

---

## 📚 Intended Audience

- SOC Analysts (L1 / L2 / L3)  
- Blue Team Engineers  
- Cyber Security Students  
- Interview and Portfolio Preparation  

---

## 📘 Detection Engineering & Framework Mapping

This repository includes detailed SOC-focused documentation:

- 🔍 **SIEM Detection Rules**  
  Practical detection logic for AI-related threats using Splunk, Microsoft Sentinel (KQL), and IBM QRadar  
  👉 [View SIEM Detections](./siem-detections.md)

- 🧠 **MITRE ATT&CK Mapping**  
  Mapping of AI security threats to MITRE ATT&CK tactics and techniques  
  👉 [View MITRE Mapping](./mitre-mapping.md)

---

## 📌 License

MIT License – Free to use and share.
