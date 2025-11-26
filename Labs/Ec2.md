# 🛡️ AWS EC2 Instance Management Guide: Web Server Deployment

This document outlines the technical procedures for launching, managing, and securing an Amazon EC2 web server instance, covering its lifecycle, termination protection, and resizing.

---

<img width="1726" height="651" alt="1" src="https://github.com/user-attachments/assets/10553e6b-8353-48b5-aa4b-eff033f619e9" />



## 1. 🌐 Accessing the EC2 Dashboard

Navigate to the **AWS Management Console** and ensure you are in the correct target region (e.g., US West - Oregon).

* **Procedure:** Locate the **EC2 service** via the "Recently visited" section or the services search bar.
* **Purpose:** The EC2 Dashboard serves as the central hub for launching instances, configuring security groups, and managing storage volumes.

---

<img width="1831" height="700" alt="2" src="https://github.com/user-attachments/assets/d644c9de-7bb5-4467-b711-aa4eb5f41d28" />


## 2. 🖥️ Instance Naming and OS Selection

Start the **"Launch instances"** configuration wizard to define the instance's identity and base environment.

| Configuration Element | Value/Selection | Description |
| :--- | :--- | :--- |
| **Name and Tags** | `Web Server` | Assign a descriptive name for easy identification. |
| **Application and OS Image** | `Amazon Linux 2023 AMI` | Provides a stable, secure, and high-performance execution environment. |

---

<img width="1848" height="762" alt="3" src="https://github.com/user-attachments/assets/e68ab0f3-cbc3-4410-b591-fcb882c67d94" />


## 3. 🔒 Network and Security Group Configuration

Proper network configuration is critical for defining firewall rules and communication with the internet.

* **VPC & Subnet:** Ensure the correct VPC and Subnet are selected for your deployment environment.
* **Security Group:** Select **"Create security group"** and name it `Web Server Security group`.
* **Firewall Rule:** This step is crucial for defining inbound rules that must allow **HTTP traffic** to reach your web server.

---

<img width="1817" height="698" alt="4" src="https://github.com/user-attachments/assets/a79e0b31-098e-4ea4-8634-bbed509e9e6e" />

## 4. ⚙️ Configuring User Data for Automated Setup (Bootstrapping)

To automate the deployment of the web server software, use the **"User data"** field within the **Advanced Details** section.

**Injected Bash Script (Example):**

```bash
#!/bin/bash
# Install Apache web server (httpd)
yum update -y
yum install -y httpd.x86_64
systemctl start httpd
systemctl enable httpd
echo '<h1>Hello from Web Server!</h1>' > /var/www/html/index.html
```
---

<img width="1553" height="737" alt="5" src="https://github.com/user-attachments/assets/0ea2cf4b-d33c-4fd1-a72b-d328f9b87db4" />


## 5. ✅ Monitoring Instance Status

Once launched, monitoring the instance's transition is essential for verification.

| Dashboard Field | Expected State | Meaning |
| :--- | :--- | :--- |
| **Instance State** | `Running` | Instance has successfully booted after being in the `Pending` state. |
| **Status check** | `3/3 checks passed` | Confirms that both the system reachability and instance connectivity are healthy and the server is ready to accept traffic. |

---

<img width="1852" height="763" alt="6" src="https://github.com/user-attachments/assets/f107d5c3-c420-47c0-b75f-1e5f713f944a" />

## 6. ⬆️ Resizing the Instance (Vertical Scaling)

To change the instance type (resize), the instance must first be in a **"Stopped"** state to modify the allocated compute resources.

| State | Action Path | Purpose | Example Scale |
| :--- | :--- | :--- | :--- |
| **Stopped** | Access the **Actions** menu. | Necessary prerequisite for modifying hardware attributes. | N/A |
| **Settings** | `Instance settings` $\rightarrow$ `Change instance type`. | Allows scaling the server (e.g., from `t3.micro` to `t3.small`) to handle increased load. | Scaling Up |

---

<img width="1548" height="405" alt="7" src="https://github.com/user-attachments/assets/c6e38fb0-49fb-4aaf-bec2-f09a258d79b5" />


## 7. 🔒 Testing Termination Protection

Termination protection prevents accidental deletion of critical resources. Attempting to terminate a protected instance validates that this safeguard is active.

* **Test Action:** Attempt to **"Terminate (delete) instance."**
* **Result:** The console triggers a specific error message confirming the security configuration successfully blocked the action:

> **Error Message:** "Failed to terminate... Modify its 'disableApiTermination' instance attribute and try again."
