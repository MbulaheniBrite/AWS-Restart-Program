
This lab focuses on using **Amazon Virtual Private Cloud (VPC)** to create a customized, isolated network environment and deploying an EC2-based web server into that environment. This exercise simulates building a customized network architecture for an enterprise customer.

---

## 🎯 Objectives and Scenario

### Objectives
After completing this lab, you should be able to:

* **Create a virtual private cloud (VPC)**.
* **Create subnets** within the VPC.
* **Configure a security group** (firewall rules).
* **Launch an Amazon Elastic Compute Cloud (Amazon EC2) instance** into a VPC.

### Scenario
In this lab, you use Amazon VPC to create your own VPC and add additional components to produce a customized network for a Fortune 100 customer. You create security groups for your EC2 instance, configure a web server, and launch it into the VPC.

---

## Technical Procedure: VPC Setup and Web Server Deployment

### 1. 🏗️ Initial VPC Creation

The process begins by setting up the isolated network environment using the automated workflow.

#### 1.1 Accessing the VPC Dashboard
The user prepares to start the VPC creation process from the **VPC dashboard**.

<img width="1060" height="226" alt="2" src="https://github.com/user-attachments/assets/5f80c3b4-e937-4d12-b990-2634775f1d39" />

#### 1.2 Defining the VPC Network
The user selects the **"VPC and more"** option to automate the creation of a standard two-tier architecture.

<img width="1861" height="882" alt="3" src="https://github.com/user-attachments/assets/c914135c-b791-44ae-bd05-0c3d349dd53a" />

* **VPC Name:** Set to `Lab VPC`.
* **IPv4 CIDR Block:** Set to $\mathbf{10.0.0.0/16}$, reserving a large private IP range.
* **Preview:** Shows the creation of Public and Private Subnets and associated Route Tables in the `us-west-2` region.


#### 1.3 Automated Resource Creation Confirmation
The **`Create VPC workflow`** confirms the successful provisioning of all foundational network resources, including:

<img width="1847" height="830" alt="4" src="https://github.com/user-attachments/assets/4eb74f0c-5ed2-442b-9904-4c9cebf0c5dc" />

* VPC Creation ($\mathbf{vpc-0431518c...}$).
* Enabled DNS hostnames and resolution.
* Subnets, Route Tables, and **Internet Gateway (IGW)** creation and attachment.


#### 1.4 Viewing Initial Subnets
The Subnets dashboard lists the four subnets automatically created by the workflow.

<img width="1216" height="638" alt="5" src="https://github.com/user-attachments/assets/07038014-0e60-41eb-acf1-1c65efb9b8c8" />

---

### 2. 🗺️ Subnet and Route Table Configuration

<img width="1787" height="743" alt="6" src="https://github.com/user-attachments/assets/70c1323e-33a8-4014-b7f9-191dba97e1d5" />

The network is customized by creating an additional subnet and explicitly associating subnets with the correct route tables.

#### 2.1 Creating an Additional Subnet
A new subnet named **`Public Subnet 2`** is created within the existing `Lab VPC` ($\mathbf{10.0.0.0/16}$).

<img width="1845" height="552" alt="7" src="https://github.com/user-attachments/assets/c6713e80-c637-4257-af5a-752bb5d5e0cf" />


#### 2.2 New Subnet Creation Confirmation
The Subnets dashboard confirms the successful creation and availability of **`Public Subnet 2`** ($\mathbf{subnet-0ada8314...}$).

<img width="1850" height="831" alt="8" src="https://github.com/user-attachments/assets/12754752-e87e-4014-bc17-4ba6bbc282f4" />

#### 2.3 Associating Public Subnets with Public Route Table
The **`Public Route Table`** (`rtb-0009f...`) is configured to associate with **`Public Subnet 2`** ($\mathbf{10.0.2.0/24}$), ensuring its public access path via the IGW.

<img width="1855" height="648" alt="9" src="https://github.com/user-attachments/assets/8ec16a2e-ce7e-4aa6-927e-6d7be45d66c3" />

#### 2.4 Associating Private Subnets with Private Route Table
The **`Private Route Table`** (`rtb-034fa...`) is configured to associate with **`Private Subnet 2`** ($\mathbf{10.0.3.0/24}$), confirming its role in isolating backend resources.

<img width="1197" height="548" alt="10" src="https://github.com/user-attachments/assets/16f58be5-0533-4a08-92eb-a36e06dd63c9" />

---

### 3. 🔒 Security Group and EC2 Launch

A dedicated security group is configured to enforce firewall rules, followed by the deployment of the web server instance.

#### 3.1 Preparing to Create Security Group
The user navigates to the **Security Groups** section and selects **"Create security group"**.

<img width="1197" height="548" alt="10" src="https://github.com/user-attachments/assets/d4c2b624-d4ff-4077-aa02-7609a84bdc1c" />

#### 3.2 Security Group Configuration Confirmation
A new Security Group named **`Web`** (`sg-0f24364f...`) is successfully created. The configuration includes:

<img width="1218" height="616" alt="11" src="https://github.com/user-attachments/assets/0c7dcac4-7a28-410d-9d2a-d80bf5b478a7" />

* **Description:** "Enable HTTP access."
* **Rules:** **1 Inbound rule** (for required HTTP traffic) and **1 Outbound rule**.


#### 3.3 Web Server Launch and Health Check
The **`Web Server 1`** EC2 instance is launched into the public subnet using the **`t3.micro`** type.

<img width="1532" height="666" alt="12 2" src="https://github.com/user-attachments/assets/8fc3bd2a-cfbc-46d4-9589-20e8fc44023e" />

* **State:** **`Running`**.
* **Status Check:** Confirmed to be fully operational with **`3/3 checks passed`**.
* **IP Addresses:** Assigned a **Public IPv4 address** ($\mathbf{35.91.193.115}$) and a **Private IPv4 address** ($\mathbf{10.0.2.9}$).


---

### 4. ✅ Web Server Validation

The final step confirms the web server's public accessibility and concludes the lab.

#### 4.1 Connectivity Instructions
The instructions detail the steps for verification:

<img width="1246" height="767" alt="12" src="https://github.com/user-attachments/assets/f1eeecb9-496b-48a4-8173-a5c90901ff73" />

1.  Wait until the instance shows **`2/2 checks passed`**.
2.  Copy the **Public IPv4 DNS** value.
3.  Paste the value into a web browser and press Enter.
   
<img width="1241" height="131" alt="13" src="https://github.com/user-attachments/assets/79ff14d8-63eb-467e-8c22-1bf8c8131592" />

#### 4.2 Lab Completion Confirmation
The successful completion of all objectives is confirmed.

