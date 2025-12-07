## 🔒 AWS Security Assessment: Amazon Inspector Workflow

## Project Goal

This documentation provides a technical walkthrough detailing the workflow for activating and utilizing **Amazon Inspector** to perform continuous, automated vulnerability management on AWS resources, focusing specifically on assessing and remediating findings within **AWS Lambda functions**.

---

## I. 🔎 Vulnerability Assessment & Remediation (Amazon Inspector)

### 1. Lab Overview and Objectives
* **Description:** Outlined the primary goal of the lab: to use Amazon Inspector to scan AWS resources (specifically Lambda functions) for vulnerabilities and remediate those findings.
* **Objectives:** **Activate Inspector**, **analyze findings**, and **remediate vulnerabilities**.

<img width="1296" height="603" alt="1" src="https://github.com/user-attachments/assets/4db554a0-1fdc-441c-a912-9c5d80b2a00d" />

### 2. Inspector Service Introduction
* **Description:** Reviewed the Amazon Inspector service page, noting its critical role as an automated and continual vulnerability management system for the AWS environment.

<img width="1821" height="831" alt="2" src="https://github.com/user-attachments/assets/45b5e1e9-1a40-4a6d-9597-f0263f0414ed" />

### 3. Activating Amazon Inspector
* **Description:** Initiated the activation process for Amazon Inspector, which grants the necessary service permissions to begin continuous vulnerability discovery across the environment.

<img width="1865" height="832" alt="3" src="https://github.com/user-attachments/assets/76926d41-349e-44ba-88d2-811d379a3b8d" />

### 4. Inspector Dashboard Summary
* **Description:** Reviewed the initial dashboard view immediately after activation, showing the scope of environment coverage and the initial status of critical findings (before the first comprehensive scan results fully populated).

<img width="1845" height="824" alt="4" src="https://github.com/user-attachments/assets/131c9b69-dd21-4a94-81dc-d0e8cf1f1eed" />

### 5. Reviewing Active Findings
* **Description:** Displayed the initial scan results, showing **three Medium** severity package vulnerabilities found within the **get-request** Lambda function.
* **Vulnerabilities Listed:** CVE-2024-35195, CVE-2024-47081, CVE-2023-32681.

<img width="1868" height="823" alt="5" src="https://github.com/user-attachments/assets/338aa260-d18c-4ab7-a538-aad18ff4d3b8" />

### 6. Detailed Finding Analysis (CVE-2023-32681)
* **Description:** Drilled down into the specifics of a single vulnerability, showing the severity, the availability of a fix, and the affected package and version (**requests 0.2.20.0**).

<img width="1860" height="832" alt="6" src="https://github.com/user-attachments/assets/be82de43-c68b-4ee4-988b-f49644ae0c3a" />

### 7. CVE-2023-32681 Description (NVD Review)
* **Description:** Reviewed the official NVD (National Vulnerability Database) detail page to confirm the nature of the vulnerability, which specifically relates to **Proxy-Authorization header leakage** in HTTP requests.

<img width="1557" height="944" alt="7" src="https://github.com/user-attachments/assets/f0d8ab5e-16e2-43af-b655-fe3fe2ba02ce" />

### 8. Lambda Function List
* **Description:** Displayed the list of the two specific Lambda functions being actively monitored by Amazon Inspector for code and package vulnerabilities.
* **Functions Monitored:** **get-request** and **generate-password-for-new-account**.

<img width="1851" height="749" alt="8" src="https://github.com/user-attachments/assets/48edb811-e15c-4013-bf48-674cdb21e516" />

### 9. Lambda Code Remediation
* **Description:** Viewed the source code of the vulnerable get-request function, which performs a simple GET request to a sample URL (contextualizing where the vulnerable library was used).

<img width="1801" height="822" alt="9" src="https://github.com/user-attachments/assets/3c0f4eeb-32a8-4b21-a84e-85b60fc15ad2" />

### 10. Verifying Remediation (Closed Findings)
* **Description:** Displayed the Inspector findings dashboard after remediation actions (such as updating the vulnerable library version) were performed.
* **Finding Status:** All 3 findings successfully transitioned to **Closed**.

<img width="1539" height="484" alt="10" src="https://github.com/user-attachments/assets/d132bf44-1b48-4529-908a-e31341f6ddb6" />

### 11. Scanning Status Check
* **Description:** Confirmed the continuous scanning status for both Lambda functions, verifying that the remediated function, **get-request**, now shows a status of **No findings**.

<img width="1548" height="823" alt="11" src="https://github.com/user-attachments/assets/ecc47461-4bf7-4242-99f3-0ece66789c6b" />

### 12. Inspector Lab Conclusion
* **Description:** Final summary confirming successful completion of all vulnerability assessment and remediation objectives outlined at the start of the lab.
* **Success:** Activated Inspector, analyzed findings, and successfully **remediated vulnerabilities**.

<img width="1147" height="328" alt="12" src="https://github.com/user-attachments/assets/88edb386-f78b-4bb0-982b-b9e45f3a638e" />
