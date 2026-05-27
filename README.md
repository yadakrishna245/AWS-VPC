# AWS VPC Setup – Production Environment

## Overview
This project documents the creation of a production-ready AWS VPC with a secure, scalable, and highly available network architecture. The setup follows best practices for isolating public, private, and database layers.

---

## VPC Configuration

- **VPC CIDR:** `192.168.50.0/24`
- Provides isolated network environment for all resources
- Designed for scalability with reserved IP space for future use

---

## Subnet Architecture

A total of **6 subnets** were created:

### Public Subnets (2)
- Used for internet-facing components
- Examples: Load Balancers, Bastion Host
- Connected to Internet Gateway

### Private Subnets (2)
- Used for application servers
- No direct internet access
- Outbound internet enabled via NAT Gateway

### Database Subnets (2)
- Used for database instances
- Completely isolated (no internet access)
- Accessible only from private subnets

### Reserved IPs
- Additional CIDR ranges kept unused for future expansion

---

## Route Tables

Three route tables were configured:

### Public Route Table
- Associated with Public Subnets
- Routes internet traffic via Internet Gateway (IGW)

### Private Route Table
- Associated with Private Subnets
- Routes outbound traffic through NAT Gateway

### Database Route Table
- Associated with DB Subnets
- No internet route for enhanced security

---

## Internet Gateway (IGW)
- Created and attached to the VPC
- Enables internet access for public subnet resources
- Added to Public Route Table

---

## NAT Gateway
- Created inside a Public Subnet
- Attached with an Elastic IP
- Allows outbound internet access for Private Subnets
- Ensures no inbound internet exposure

---

## Security Architecture

- **Public Layer**
  - Internet-accessible resources
  - Controlled via Security Groups

- **Private Layer**
  - Application servers
  - Acts as jump host / bastion access layer
  - No direct inbound internet access

- **Database Layer**
  - Fully private
  - Accessible only within VPC (from application layer)

---

## Key Benefits

- ✅ Network isolation between tiers  
- ✅ Secure internet access control  
- ✅ High availability ready (multi-AZ support)  
- ✅ Scalable subnet planning  
- ✅ Production-grade architecture  

---

## Future Improvements

- Add VPC Endpoints for private AWS service access
- Enable VPC Flow Logs for monitoring
- Implement Network ACLs for extra security
- Configure VPN / Direct Connect for hybrid setup

---


## Summary
This setup ensures a strong foundation for deploying production workloads in AWS with clearly separated layers, controlled access, and scalability in mind.



## Interview Questions & Answers

### 1. What is a VPC?
A VPC (Virtual Private Cloud) is a logically isolated network in AWS where resources can be launched securely with full control over IP addressing, routing, and security.

---

### 2. Why did you use 3 types of subnets?
- **Public Subnets** → For internet-facing resources  
- **Private Subnets** → For application servers  
- **DB Subnets** → For databases with no internet access  
This ensures **layered security (3-tier architecture)**.

---

### 3. What is the purpose of an Internet Gateway?
It allows communication between VPC resources and the internet. It is attached to the VPC and used in public route tables.

---

### 4. Why do we need a NAT Gateway?
A NAT Gateway allows **private subnet instances to access the internet outbound only** (e.g., updates, patches) without exposing them to inbound traffic.

---

### 5. Why are databases placed in private subnets?
For **security reasons**. Databases should not be exposed to the internet and should be accessed only from application servers inside the VPC.

---

### 6. What is the difference between Public and Private Subnets?
- **Public Subnet** → Has route to Internet Gateway  
- **Private Subnet** → No direct internet route  

---

### 7. What are Route Tables?
Route tables define how traffic flows inside the VPC. Different route tables are used to control access for public, private, and DB layers.

---

### 8. What is a Bastion Host?
A bastion host (jump server) is placed in a public subnet and used to securely connect to private instances.

---

### 9. Why did you reserve IP ranges?
Reserved IP ranges help in **future scalability**, avoiding conflicts and enabling easy expansion without redesign.

---

### 10. How is high availability achieved?
By deploying subnets across multiple availability zones and distributing resources across them.

---

### 11. How do you secure this architecture?
- Security Groups (instance-level firewall)  
- Network ACLs (subnet-level control)  
- No direct DB internet access  
- Controlled access via NAT and Bastion Host  

---

### 12. Difference between Security Groups and NACL?
- **Security Groups** → Stateful  
- **NACLs** → Stateless  

---

### 13. Can private instances be accessed from internet?
No. They can only access the internet outbound via NAT Gateway, but cannot be accessed directly from outside.

---

### 14. Why not use a single subnet?
Using multiple subnets improves:
- Security
- Availability
- Fault isolation
- Scalability

---

### 15. What improvements can be made?
- Add Load Balancer (ALB)
- Auto Scaling
- VPC Endpoints
- Monitoring (CloudWatch, VPC Flow Logs)


