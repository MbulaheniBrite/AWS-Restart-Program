## 🛠️ My AWS VPC Networking Troubleshooting - Service Note

## Quick Overview

**Service Date:** 09-11-2025

**Service Type:** VPC Network Configuration and Troubleshooting  
**Scenario:** Cloud Support Engineer Simulation  
**Customer:** Startup Company  
**Completed By:** Brite Sendedza  
**Status:** Successfully Resolved

---

## 🎯 What I Was Trying to Do

I played the role of a **Cloud Support Engineer at AWS**, helping a startup company fix a networking issue in their newly created AWS infrastructure. My job was to build, configure, and troubleshoot a routable network environment within an Amazon VPC so their EC2 instance could communicate with the internet.

**The Goal:** Get their EC2 instance to successfully ping external addresses, proving that outbound internet connectivity was working properly.

---

## The AWS Services I Worked With

### Amazon VPC (Virtual Private Cloud)
**What It Is:** VPC is like having your own private data center in the cloud. It's an isolated network where you can launch AWS resources with complete control over IP addressing, subnets, and routing.

**How I Used It:** I created a VPC as the foundation for the customer's network infrastructure. Everything else—subnets, gateways, instances—lives inside this VPC.

### Internet Gateway (IGW)
**What It Is:** An Internet Gateway is basically the door between your VPC and the public internet. Without it, resources in your VPC can't communicate with anything outside.

**How I Used It:** I attached an IGW to the VPC to enable outbound and inbound internet connectivity for resources in public subnets.

### Route Table
**What It Is:** Route tables are like GPS directions for your network traffic. They tell traffic where to go based on its destination.

**How I Used It:** I configured route tables to direct internet-bound traffic (0.0.0.0/0) to the Internet Gateway, making outbound connectivity possible.

### Security Group (SG)
**What It Is:** Security Groups act as virtual firewalls at the instance level. They control inbound and outbound traffic using allow rules.

**How I Used It:** I configured the security group to allow the necessary traffic types—like ICMP for ping and SSH for remote access.

### Network Access Control List (NACL)
**What It Is:** NACLs are another layer of security that works at the subnet level. Unlike Security Groups, they use both allow and deny rules.

**How I Used It:** I verified and configured NACL rules to ensure they weren't blocking the traffic I needed—particularly ICMP echo requests and responses for ping.

### Amazon EC2 (Elastic Compute Cloud)
**What It Is:** EC2 provides virtual servers in the cloud that you can configure and use for pretty much anything.

**How I Used It:** I launched an EC2 instance in the VPC to test network connectivity and validate that my configuration was working.

---

## What I Actually Did

### 1. Understanding the Customer's Problem
First, I reviewed the customer's scenario. They had created AWS infrastructure but were experiencing networking issues—their EC2 instance couldn't communicate with the internet.

### 2. Building the VPC Foundation
I created a new VPC with a proper CIDR block to establish the private network space for their resources.

### 3. Setting Up the Internet Gateway
I created an Internet Gateway and attached it to the VPC. This is what enables internet connectivity.

### 4. Configuring Route Tables
I set up route tables and added a route pointing internet-bound traffic (0.0.0.0/0) to the Internet Gateway. Without this, traffic wouldn't know how to reach the internet.

### 5. Creating Security Groups
I configured a Security Group with rules to allow:
* ICMP traffic (for ping testing)
* SSH traffic (for remote access)
* Any other necessary protocols

### 6. Configuring Network ACLs
I checked and adjusted the NACL rules to ensure they weren't blocking traffic. NACLs can be tricky because they're stateless—you need rules for both inbound and outbound traffic.

### 7. Launching the EC2 Instance
I launched an EC2 instance in a public subnet within the VPC, making sure it had a public IP address assigned.

### 8. Testing Connectivity
I connected to the EC2 instance and attempted to ping external addresses to verify internet connectivity.

---

## Challenge I Faced

### Pinging the EC2 Didn't Return Expected Results

**The Problem:** When I tried to ping external addresses from my EC2 instance, I wasn't getting the responses I expected. The pings were either timing out or failing completely, which meant something was blocking the traffic.

**Why This Was Frustrating:** Everything looked configured correctly on the surface—I had the IGW attached, route tables set up, and security groups configured. But something was still preventing the ping from working.

---

## How I Fixed It

**The Solution:**
* **Checked Security Group Rules:** I verified that my security group allowed outbound ICMP traffic (it does by default, but worth checking) and made sure inbound ICMP was allowed for the echo replies to come back.
* **Fixed NACL Rules:** The culprit was the Network ACL! NACLs are stateless, so I needed explicit rules for both outbound ICMP echo requests AND inbound ICMP echo replies. I added rules allowing ICMP traffic in both directions.
* **Verified Route Table:** I double-checked that my route table had the correct route (0.0.0.0/0 → IGW) and that it was associated with the correct subnet.
* **Public IP Assignment:** I confirmed my EC2 instance had a public IP address—without it, return traffic wouldn't know where to go.

Once I fixed the NACL rules, the pings started working immediately!

---

## Results

| Test | Expected Result | Actual Result | Status |
| :--- | :--- | :--- | :--- |
| Ping to external IP | Successful echo replies | Successful echo replies | Pass |
| Outbound internet access | Connection established | Connection established | Pass |
| Route table configuration | Traffic routed to IGW | Traffic routed to IGW | Pass |
| Security Group rules | ICMP allowed | ICMP allowed | Pass |
| NACL rules | Both directions allowed | Both directions allowed | Pass |

**Key Achievement:** Successfully configured a fully functional VPC network with verified internet connectivity.

---

## 🎓 What I Learned

### Security Groups vs. NACLs
The big lesson here was understanding the difference between Security Groups and NACLs:
* **Security Groups** are stateful (return traffic is automatically allowed)
* **NACLs** are stateless (you need explicit rules for both directions)

This is a common gotcha that trips people up!

### Layer by Layer Troubleshooting
I learned to troubleshoot networking issues systematically:
1. Check routing (Is there a path to the internet?)
2. Check security groups (Is traffic allowed at the instance level?)
3. Check NACLs (Is traffic allowed at the subnet level?)
4. Check the instance itself (Does it have a public IP?)

### The Importance of Public IPs
Without a public IP address, EC2 instances in public subnets can send traffic out but can't receive responses. This was a key thing to verify.

---

## What I'd Recommend for Production

* **Use VPC Flow Logs:** Enable flow logs to track and troubleshoot network traffic
* **Implement Bastion Hosts:** For secure SSH access instead of direct internet exposure
* **Document Network Architecture:** Keep clear diagrams of subnets, route tables, and security configurations
* **Follow Least Privilege:** Only open the ports and protocols that are absolutely necessary
* **Regular Security Audits:** Periodically review security group and NACL rules

---

## ✍️ Final Thoughts

**Issue Status:** Resolved  
**Customer Satisfaction:** Network fully operational  
**Skills Gained:** VPC troubleshooting, NACL configuration, systematic debugging

**My Closing Notes:** This lab was a great simulation of real-world cloud support work. The customer's networking issue came down to NACL configuration—a common but easily fixable problem once you understand how stateless firewalls work. I successfully built a complete VPC network from scratch, troubleshot connectivity issues, and got everything working. The ability to ping external addresses confirmed that the network was properly configured with internet access. This is exactly the kind of systematic troubleshooting that Cloud Support Engineers do every day!

**Completed On:** 09-11-2025  
**By:** Brite Sendedza
