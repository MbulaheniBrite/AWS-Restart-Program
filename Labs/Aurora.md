# How I Deployed My AWS Aurora (PostgreSQL) Database

## What This Project Is About

In this document, I'm walking through the entire process of deploying an **AWS Aurora (PostgreSQL Compatible)** database cluster. I'll cover everything from initial setup and network security to provisioning the database, connecting to it from an EC2 instance, and running SQL queries to interact with my data.

---

## Part I. Setting Up and Configuring My Database

### 1. Getting to the Console
* I accessed the **Aurora and RDS** service from the AWS Console to kick things off
* I made sure I was in the right region
* Region: **US West (Oregon)**

<img width="917" height="355" alt="A" src="https://github.com/user-attachments/assets/98adec21-fc87-4274-887c-5e5fba1e8acb" />

### 2. Starting the Database Creation
* I opened up the empty **Databases** dashboard
* I clicked **Create database** to get started

<img width="915" height="352" alt="B" src="https://github.com/user-attachments/assets/460ce969-6749-4a13-acb1-d8b062f5c29e" />

### 3. Choosing My Database Engine
* I selected the full configuration method so I could customize everything
* I picked the database technology I wanted to use
* Engine Type: **Aurora (PostgreSQL Compatible)**

<img width="916" height="355" alt="C" src="https://github.com/user-attachments/assets/b5d71697-2b26-4967-822b-a32d8fc8b139" />

### 4. Picking the Template and Version
* I defined what I'd be using this database for
* I selected the specific PostgreSQL-compatible engine version I needed
* Template: **Dev/Test**
* Version: **PostgreSQL 11.9**

<img width="918" height="354" alt="D" src="https://github.com/user-attachments/assets/b7ce04de-277d-4bf6-a22c-8d93b53e2970" />

### 5. Setting Up My Login Credentials
* I configured the master access credentials for my database
* Master Username: **admin**
* Password Management: **Self managed**

<img width="911" height="352" alt="E" src="https://github.com/user-attachments/assets/088612c5-3484-4189-a9c0-14cdab76418c" />

### 6. Configuring the Instance Size
* I specified how much compute power I wanted for my Aurora instance
* I decided whether I needed high availability with replicas
* Class: **db.t3.medium**
* Multi-AZ: **Don't create an Aurora Replica**

<img width="914" height="354" alt="F" src="https://github.com/user-attachments/assets/900b5063-98fe-41cd-bccd-47fe827a0927" />

### 7. Placing It in My Network
* I assigned my database to the right VPC and subnet group
* This ensures proper network isolation
* VPC: **LabVPC**
* Subnet Group: **dbsubnetgroup**

<img width="916" height="355" alt="G" src="https://github.com/user-attachments/assets/05d66d14-7aaa-4cac-b31c-bb73fcbaf610" />

### 8. Setting Up Security (My Firewall Rules)
* I configured access control to make sure my database is only reachable from within the VPC
* No public internet access for extra security
* Public Access: **No**
* Security Group: **DBSecurityGroup**

<img width="914" height="356" alt="H" src="https://github.com/user-attachments/assets/a82d3b22-b372-436a-b1ab-152f46d127be" />

### 9. Naming My Database
* I defined the name of my initial database schema within the cluster
* Initial DB Name: **world**

<img width="902" height="349" alt="I" src="https://github.com/user-attachments/assets/425b6c7f-f9fd-443a-8a0d-64efda295159" />

### 10. Reviewing Maintenance and Costs
* I reviewed the automated settings before launching
* I checked the estimated monthly cost to make sure it fit my budget
* Deletion Protection: **Disabled**
* Estimated Cost: **$61.86 USD**

<img width="852" height="356" alt="J" src="https://github.com/user-attachments/assets/8805eb8a-fadb-4614-85d6-29448de8365d" />

### 11. Watching It Get Created
* I confirmed that the deployment process started
* Status: **Creating**
* Now I just had to wait for it to finish provisioning

<img width="917" height="360" alt="K" src="https://github.com/user-attachments/assets/b873adb2-eda5-49ff-81bd-5eca39a2faa6" />

## Part II. Setting Up My Client and Connecting

### 12. Checking My EC2 Command Host
* I verified that my pre-created EC2 instance was ready to go
* This is the machine I'd use to connect to my private database
* Name: **Command Host**
* Status: **Running, 3/3 checks passed**

<img width="916" height="353" alt="L" src="https://github.com/user-attachments/assets/1dedf686-e5f5-4806-8c1a-0c9876e9b79e" />

### 13. Connecting to My EC2 Instance
* I initiated the connection to my EC2 host
* I used the browser-based client to make things easier
* Tool: **Session Manager**

<img width="920" height="353" alt="M" src="https://github.com/user-attachments/assets/fa2c13ce-a9c8-4f43-aa73-d3e362a8f9a2" />

### 14. Installing the Database Client
* I ran a command on my EC2 host to install the client package I needed
* This would let me connect to and interact with my database
* Command: sudo yum install mariadb -y

<img width="591" height="338" alt="N" src="https://github.com/user-attachments/assets/7f45dc29-0cd1-4c86-b073-e2a6b5e67fd8" />

### 15. Confirming My Cluster Was Ready
* I checked to make sure my Aurora cluster and primary instance were fully deployed
* They were ready for connections!
* Status: **Available**

<img width="919" height="352" alt="O" src="https://github.com/user-attachments/assets/05ae5a3b-9ff6-4c07-8b9b-69a938d62c77" />

### 16. Finding My Connection Details
* I looked at the connection endpoints
* I identified which endpoint to use for reading and which for writing
* Endpoints: **Writer** and **Reader**
* Port: **5432**

<img width="920" height="357" alt="P" src="https://github.com/user-attachments/assets/d71969ad-5bba-4f9c-a219-58a20411d77f" />

## Part III. Working With My Data

### 17. Creating My First Table
* I prepared and executed the SQL command to create my **country** table
* I defined all the columns I needed
* I set up a **PRIMARY KEY** on the Code column

<img width="681" height="302" alt="Q" src="https://github.com/user-attachments/assets/b619f8f7-011b-4643-9df0-133d24fdb0bb" />

### 18. Writing My Query
* I prepared an SQL query to filter my database records
* I wanted to find countries that met specific financial and demographic criteria
* My criteria: GNP > 35000 AND Population > 10000000

<img width="680" height="71" alt="R" src="https://github.com/user-attachments/assets/43567f8f-f0d6-4612-b227-9e05c4e15c40" />

### 19. Checking My Results
* I looked at the data that matched my query conditions
* Records that came back: **Australia** and **Thailand**
* Exactly what I was looking for!

<img width="697" height="197" alt="S" src="https://github.com/user-attachments/assets/7cbf0c4e-d15d-49f9-9ce1-973bbc359051" />

### 20. Lab Complete!
* I wrapped everything up with a final check
* I successfully completed all my database objectives
* **Success:** My Aurora instance was created, connected, configured, and I could query it successfully!

<img width="700" height="200" alt="T" src="https://github.com/user-attachments/assets/1573327e-5ba1-4018-915a-893962a4ec2c" />
