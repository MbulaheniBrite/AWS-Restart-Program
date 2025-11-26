<img width="1126" height="737" alt="28" src="https://github.com/user-attachments/assets/cf50951f-746f-4e2d-9d60-d420d797423a" />☁️ AWS Sales Reporting System Deployment: Step-by-Step Guide

This document chronicles the step-by-step deployment and configuration of a fully serverless, automated sales reporting pipeline using AWS services, detailing IAM roles, Lambda function creation, VPC integration, and scheduling.

Part 1. 🏗️ Initial Setup and IAM Role Creation

1. 🔍 System Architecture Diagram

This diagram outlines the complete architecture, showing the data flow from the EventBridge trigger through the Lambda functions and VPC-hosted database to the final SNS topic notification.

<img width="987" height="552" alt="1" src="https://github.com/user-attachments/assets/9b118b3b-270d-4caa-bc1e-cf3b8e48063b" />

2. 👤 IAM Role Overview

<img width="1025" height="728" alt="1 1" src="https://github.com/user-attachments/assets/707706d0-de56-4c20-8c90-13f5cbd7ebe4" />

The IAM console is accessed to begin the creation of the necessary service role. The role, ultimately named salesAnalysisReportRole, will grant the Lambda functions permissions to interact with other AWS services.

3. 🤝 Configuring Trust Policy for Lambda

The "Trusted entity type" is set to AWS service, and the use case is specified as Lambda. This configuration grants the AWS Lambda service the authority to assume this role when executing a function.

<img width="1150" height="740" alt="3" src="https://github.com/user-attachments/assets/1a593412-0492-4ced-84fb-b333818cb151" />

4. ✅ Role Creation Confirmation

The final screen confirms the creation of the IAM role. Key details, such as the Role Name (salesAnalysisReportRole) and the established Trust relationship, are displayed, verifying that the Lambda service is the permitted trusted entity.

<img width="1153" height="736" alt="6" src="https://github.com/user-attachments/assets/c18f7689-f19f-44e6-a080-1913d531c7e8" />

Part 2. 🧰 Lambda Function and Dependency Management

This section details the deployment of the Lambda function and the preparation of its dependencies via Lambda Layers.

5. 💻 Lambda Console Access

The AWS Lambda console dashboard is displayed, showing the environment where the serverless function will be created.

<img width="1150" height="732" alt="5" src="https://github.com/user-attachments/assets/255f3520-3d28-4957-90dd-02bdf307fd17" />

6. 📦 Lambda Layer Creation for Dependencies

The Lambda Layers section is accessed to create a custom layer. This is necessary to package external Python libraries, such as the database connector (PyMySQL), for use within the Lambda runtime environment.

<img width="1153" height="736" alt="6" src="https://github.com/user-attachments/assets/851a5b8c-5fd4-4941-9727-baa8226c60a6" />

7. ⚙️ Layer Configuration and Runtime Selection

The new layer, named pymysqlLibrary, is configured. The package is uploaded and specified as compatible with the Python 3.9 runtime, matching the intended Lambda function's environment.

<img width="1138" height="742" alt="7" src="https://github.com/user-attachments/assets/0a6ab24b-fc0c-4ff7-98ba-9fd73593937e" />

8. 🚀 Layer Deployment Success

The success screen confirms that the pymysqlLibrary layer has been successfully created with its Amazon Resource Name (ARN), making the PyMySQL driver available for import by the function.

<img width="1151" height="727" alt="8" src="https://github.com/user-attachments/assets/6feaacf5-1978-44fb-babb-37dbe45feff3" />

9. ➕ Lambda Function Initialization

The creation of the primary data extraction Lambda function, salesAnalysisReportDataExtractor, is initiated using the "Author from scratch" method.

<img width="1156" height="730" alt="9" src="https://github.com/user-attachments/assets/265c90fa-76c8-43e2-b266-7fb70bdfc3d6" />

10. 📝 Function Configuration Details

The function is configured with the following key settings: Runtime set to Python 3.9, and the existing salesAnalysisReportRole selected as the execution role, granting it initial permissions.

<img width="1155" height="737" alt="10" src="https://github.com/user-attachments/assets/e9b82a0a-1c83-45a9-b5b2-911e04f34fb9" />

Part 3. 🌐 VPC and Core Network Infrastructure

This section covers the creation of the dedicated virtual network required for Lambda's private database access.

11. 🏗️ VPC Creation Process Initiation

Before launching the function, the user is navigated to the VPC console to initiate network setup. This shows the start of the VPC creation process, likely using the "VPC and more" option.

<img width="1156" height="672" alt="18" src="https://github.com/user-attachments/assets/783dfccc-ad9b-40fc-beb3-a2a6f147aebb" />

12. 🏷️ Configuring VPC CIDR and Naming

The VPC is configured with an IPv4 CIDR block of 10.0.0.0/16 and is named Lab VPC. This step defines the primary private IP address range for the network.

<img width="1848" height="767" alt="19" src="https://github.com/user-attachments/assets/68c38df6-ed1a-469c-ba4e-ecbc9cbed92d" />

13. 🗺️ Subnet and Availability Zone Selection

The creation process confirms the provisioning of multiple subnets (both public and private) across different Availability Zones, establishing the two-tier network design critical for isolating the database.

<img width="1150" height="731" alt="13" src="https://github.com/user-attachments/assets/3feb7810-d6e2-41a3-9a2a-5ba40d04f4af" />

14. 🚪 Internet Gateway and NAT Gateway Configuration

This confirms the setup includes an Internet Gateway (IGW) for public access and a NAT Gateway deployed in a public subnet. The NAT Gateway is crucial for allowing the Lambda function (in a private subnet) to make outbound connections to AWS services (like SNS).

<img width="1155" height="736" alt="14" src="https://github.com/user-attachments/assets/2f109dc6-c855-4e44-8924-9789b2de53cb" />

15. 🧐 Final VPC Review

The review screen summarizes all the components to be created: the VPC, public subnets, private subnets, IGW, and NAT Gateway.

<img width="1157" height="727" alt="15" src="https://github.com/user-attachments/assets/a32e9206-836d-402e-abbf-3d72f5be811f" />

16. ✅ VPC Creation Completion

The success screen confirms the completion of the VPC creation process. The key resources, including the VPC ID (vpc-0431518c...) and subnets, have been provisioned successfully.

<img width="1155" height="725" alt="16" src="https://github.com/user-attachments/assets/a4dd3e08-d951-460d-88d7-267faed82864" />

Part 4. 🔒 EC2 Host and Security Group Setup

This section details the hosting environment for the MariaDB database and the application of firewall rules.

17. 🌐 Security Group Creation for Web Access

A new Security Group named Web is created within the VPC. This group is intended for internet-facing resources, such as the EC2 host for the MariaDB database.

<img width="1155" height="732" alt="17" src="https://github.com/user-attachments/assets/2d9b4880-b643-4a18-97a5-f3bcc0b14d2e" />

18. ➡️ Inbound Rule Configuration for Web Security Group

The Web Security Group's inbound rules are configured to permit necessary external access: SSH (Port 22), HTTP (Port 80), and HTTPS (Port 443) traffic are permitted from anywhere (0.0.0.0/0).

<img width="1156" height="672" alt="18" src="https://github.com/user-attachments/assets/9e6299db-e7b9-4b9d-875e-34788922b02e" />

19. 🛡️ Database Access Security Group Creation

A second Security Group, Database, is created. This group will strictly control access to the MariaDB instance on port 3306.

<img width="1848" height="767" alt="19" src="https://github.com/user-attachments/assets/2107f117-244c-428f-9a4b-1965a0869956" />

20. ⛔ Database Inbound Rule Configuration (Zero-Trust)

The inbound rule for the Database Security Group is defined. It allows MySQL/Aurora (Port 3306) traffic, but the source is restricted to the Web Security Group's ID (sg-0f243...). This enforces a secure, zero-trust communication path, ensuring only the necessary web/DB host and the attached Lambda function can connect.

<img width="1846" height="771" alt="20" src="https://github.com/user-attachments/assets/44fcc952-a2df-4ad4-bf23-743a0d2fa8ea" />

21. 🖥️ EC2 Instance Launch for Web Server

The EC2 launch wizard is used to launch an instance named Web Server 1. This instance will host the MariaDB database required for the sales reporting system.

<img width="1137" height="665" alt="21" src="https://github.com/user-attachments/assets/3201140e-4cdd-4a6f-82e1-18dd41a98401" />

22. ✅ EC2 Instance Status Verification

The EC2 dashboard view confirms the Web Server 1 instance is in the Running state and shows the Instance ID (i-02f8a74b...) and Public IPv4 address.

<img width="806" height="247" alt="22" src="https://github.com/user-attachments/assets/5e2642fc-9f2e-475c-8a14-af5ebc437d18" />

23. 🛠️ Deployment of CLI Host

A second EC2 instance, the CLI Host, is launched. This instance serves as the administrative machine for uploading code, configuring the database, and other command-line operations.

<img width="1121" height="646" alt="23" src="https://github.com/user-attachments/assets/7dd8bc06-c2b0-4926-b6cb-bfd483bb8c5f" />

24. ☑️ CLI Host Status Verification

The EC2 dashboard confirms that the CLI Host (i-0e022e0c...) is in the Running state, ready for administrative access.

<img width="1150" height="747" alt="24" src="https://github.com/user-attachments/assets/a66b384e-9cde-4f31-9754-cc0ccbab9acc" />

Part 5. 🚀 Serverless Pipeline Orchestration and Validation

This final section covers the configuration of the Lambda function's networking, the creation of the SNS topic, and the scheduling of the automated report trigger.

25. 📎 IAM Role Access Policy Attachment

Returning to the salesAnalysisReportRole IAM role, the AmazonSNSFullAccess policy is attached. This grants the Lambda function the required permissions to publish messages to the SNS topic.

<img width="1131" height="801" alt="25" src="https://github.com/user-attachments/assets/318ce7ab-2286-4473-be1f-a48d743dd99c" />

26. 🔗 Attaching Lambda to VPC

The network configuration for the Lambda function is modified to connect it to the VPC. The function is attached to Private Subnet 1 (subnet-0f46c...) and the Database Security Group (sg-0e360...). This is essential for enabling the Lambda function to establish a private connection to the MariaDB instance.

<img width="1153" height="800" alt="26" src="https://github.com/user-attachments/assets/bdc8fc22-6abf-454d-b04e-e993511dadb5" />

27. 📧 SNS Topic Creation

An Amazon Simple Notification Service (SNS) topic, named salesAnalysisReportTopic, is created. This topic will serve as the publication channel for the final generated sales report.

<img width="1131" height="737" alt="27" src="https://github.com/user-attachments/assets/2aef00e9-b52a-483b-9cda-333014745992" />

28. ⏰ EventBridge (CloudWatch Event) Scheduler

An EventBridge rule is configured to act as the scheduler. The rule uses a cron expression (cron(0 16 ? * MON-SAT *)) to trigger the Lambda function automatically every day (Monday through Saturday) at 4:00 PM UTC.

![Uploading 28.png…]()

29. 💡 Final Report Validation (Initial Configuration)

This shows an initial view of the function after creation, possibly highlighting the basic code editor or environment variables section.

<img width="1165" height="727" alt="29" src="https://github.com/user-attachments/assets/ce6b6c0b-75b1-4d08-bd8f-7e13836b3f66" />

30. 🗺️ Lambda VPC Configuration Summary

A summary view of the Lambda function's configuration, highlighting its successful attachment to the VPC, including the assigned subnets and security groups.

<img width="1162" height="727" alt="30" src="https://github.com/user-attachments/assets/9497d66f-8b5d-434f-9976-b82cb182e5bf" />

31. 📜 SNS Topic Access Policy

The access policy for the salesAnalysisReportTopic is reviewed, ensuring that the salesAnalysisReportRole has the necessary permissions to publish messages to the topic.

<img width="897" height="335" alt="31" src="https://github.com/user-attachments/assets/0435995c-e827-4daf-ab17-1430529d2d74" />

32. ▶️ EventBridge Target Confirmation

The final EventBridge rule confirmation screen, showing the schedule and verifying that the target of the rule is the salesAnalysisReportDataExtractor Lambda function.

<img width="1817" height="698" alt="32" src="https://github.com/user-attachments/assets/a79e0b31-098e-4ea4-8634-bbed509e9e6e" />

33. ✅ Final Report Validation

The final screenshot displays the successful output of the sales report payload. This confirms that the entire automated pipeline—from scheduled trigger, Lambda execution, data extraction, report formatting, and successful publishing to SNS—is fully operational.

<img width="1162" height="727" alt="33" src="https://github.com/user-attachments/assets/0c2e36f4-a834-4d04-b8ac-17a7a618ec21" />
