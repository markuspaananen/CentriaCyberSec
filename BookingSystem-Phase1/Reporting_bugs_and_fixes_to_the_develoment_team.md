# 1️⃣ Introduction

**Tester(s):**  
- Name:  Markus Paananen

**Purpose:**  
- Identify vulnerabilities in registration and authentication flows.

**Scope:**  
- Tested components:  http://localhost:8000/register
- Exclusions:  none
- Test approach: Gray-box

**Test environment & dates:**  
- Start:  24.11.2025
- End:  24.11.2025
- Test environment details (OS, runtime, DB, browsers): Kali linux wm + docker inside the wm. zap crome

**Assumptions & constraints:**  
- No access to backend code.
- Limited to the registration endpoint.

---

# 2️⃣ Executive Summary

**Short summary:**  
Running the zap first in start here --> attack, then running active scan 2 times.

**Overall risk level:** High

**Top 5 immediate actions:**  
1.  Fix SQL injection in registration
2.  Fix Path travelsal in registration
3.  Add Anti-csrf tokens
4.  Set CSP header
5.  Fix error/warnings

---

# 3️⃣ Severity scale & definitions

|  **Severity Level**  | **Description**                                                                                                              | **Recommended Action**           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
|      🔴 **High**     | A serious vulnerability that can lead to full system compromise or data breach (e.g., SQL Injection, Remote Code Execution). | *Immediate fix required*         |
|     🟠 **Medium**    | A significant issue that may require specific conditions or user interaction (e.g., XSS, CSRF).                              | *Fix ASAP*                       |
|      🟡 **Low**      | A minor issue or configuration weakness (e.g., server version disclosure).                                                   | *Fix soon*                       |
| 🔵 **Info** | No direct risk, but useful for system hardening (e.g., missing security headers).                                            | *Monitor and fix in maintenance* |


---

# 4️⃣ Findings

> Fill in one row per finding. Focus on clarity and the most important issues.

| ID | Severity | Finding | Description | Evidence / Proof |
|------|-----------|----------|--------------|------------------|
| F-01 | 🔴 High | SQL Injection in registration | foo-bar@example.com AND 1=1 -- | ZAP alert ID 40018|
| F-02 | 🔴 High | Path travelsal in registration | Path Traversal| ZAP alert ID 6 |
| F-03 | 🟠 Medium | No Anti-CSRF tokens were found in a HTML submission form | Absence of Anti-CSRF Tokens | ZAP alert ID 10202 |
| F-04 | 🟠 Medium | Content Security Policy (CSP) Header Not Set | Content Security Policy can prevent esim. cross site scripting | ZAP alert ID 10038 |
| F-05 | 🟠 Medium | Missing Anti-clickjacking Header | CThe response does not protect against 'ClickJacking' attacks. | ZAP alert ID 10020 |
| F-06 | 🟡 Low | Application Error Disclosure | This page contains an error/warning message that may disclose sensitive information. | ZAP alert ID 90022|

---

# 5️⃣ OWASP ZAP Test Report (Attachment)

---

**Instructions (CMD version):**
1. Run OWASP ZAP baseline scan:  
   ```bash
   zap-baseline.py -t https://example.com -r zap_report_round1.html -J zap_report.json
   ```
2. Export results to markdown:  
   ```bash
   zap-cli report -o zap_report_round1.md -f markdown
   ```
3. Save the report as `zap_report_round1.md` and link it below.

---
> [!NOTE]
> 📁 **Attach full report:** → `check itslearning` → **https://github.com/markuspaananen/CentriaCyberSec/blob/main/BookingSystem-Phase1/zap_report_round1.md**

> [!NOTE]
> 📁 **Attach full report:** → `check itslearning` → **https://github.com/markuspaananen/CentriaCyberSec/blob/main/BookingSystem-Phase1/zap_report_round2.md**
---


# 6 Reporting bugs and fixes to the develoment team.
|f-01| Fixed | Did not found the finding in zap report 2| Zap alert ID 40018| 

The issue has been verified to no longer be of existance according to the required tool Zap. The report has been found not to contain any issue of id 40018.

|f-02| Fixed | Did not found the finding in zap report 2| Zap alert ID 6| 

The issue has been verified to no longer be of existance according to the required tool Zap. The report has been found not to contain any issue of id 6.

|f-03| Not fixed | Did find the finding in zap report 2| Zap alert ID 10202|

The Zap tool has flagged that issue continues to exist in the Zap report 2. as the requirements of the testing require this tool use, so has it been determined that issue still exist.

|f-04| Fixed | Did not found the finding in zap report 2| Zap alert ID 10038|

The issue has been verified to no longer be of existance according to the required tool Zap. The report has been found not to contain any issue of id 10038.

|f-05| Fixed | Did not found the finding in zap report 2| Zap alert ID 10020|

The issue has been verified to no longer be of existance according to the required tool Zap. The report has been found not to contain any issue of id 10020.

|f-01| Fixed | Did not found the finding in zap report 2| Zap alert ID 90022| 

The issue has been verified to no longer be of existance according to the required tool Zap. The report has been found not to contain any issue of id 90022.
