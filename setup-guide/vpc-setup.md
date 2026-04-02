# VPC Setup (Complete Step-by-Step Guide)

---

## 📌 Overview

This VPC is designed to support a 3-tier architecture with proper network isolation, security, and controlled internet access.

Architecture includes:
- Public Subnet (Web Layer)
- Private Subnet (Application Layer)
- Private Subnet (Database Layer)

---

## 🧱 Step 1: Create VPC

1. Go to AWS Console → VPC
2. Click **Create VPC**
3. Select **VPC only**

### Configuration:
- Name tag: my-vpc
- IPv4 CIDR: 10.0.0.0/16
- Tenancy: Default

Click **Create VPC**

---

## 🌐 Step 2: Create Subnets

Go to **Subnets → Create subnet**

---

### 🔹 Public Subnet (Web Layer)

- Name: public-subnet-1
- VPC: my-vpc
- Availability Zone: ap-south-1a
- CIDR: 10.0.1.0/24

---

### 🔹 Private Subnet (Application Layer)

- Name: private-subnet-app
- AZ: ap-south-1a
- CIDR: 10.0.2.0/24

---

### 🔹 Private Subnet (Database Layer)

- Name: private-subnet-db
- AZ: ap-south-1b
- CIDR: 10.0.3.0/24

Click **Create Subnets**

---

## 🌍 Step 3: Enable Auto-Assign Public IP (IMPORTANT)

1. Select **public-subnet-1**
2. Click **Actions → Edit subnet settings**
3. Enable:
   ✅ Auto-assign public IPv4 address

Click Save

---

## 🌐 Step 4: Create Internet Gateway

1. Go to **Internet Gateways**
2. Click **Create Internet Gateway**

### Config:
- Name: my-igw

Click Create

---

### Attach IGW to VPC

1. Select IGW
2. Click **Attach to VPC**
3. Choose: my-vpc

---

## 🧭 Step 5: Create Route Tables

---

### 🔹 Public Route Table

1. Go to **Route Tables → Create**
- Name: public-rt
- VPC: my-vpc

---

### Add Route:

1. Select public-rt
2. Go to **Routes → Edit routes → Add route**

- Destination: 0.0.0.0/0  
- Target: Internet Gateway (my-igw)

Save

---

### Associate Public Subnet

1. Go to **Subnet associations**
2. Select:
   ✅ public-subnet-1

---

## 🔹 Private Route Table

1. Create route table:
- Name: private-rt
- VPC: my-vpc

---

## 🚀 Step 6: Create NAT Gateway

1. Go to **NAT Gateways → Create**
- Name: my-nat
- Subnet: public-subnet-1
- Allocate Elastic IP

Click Create

⏳ Wait until Available

---

## 🔁 Step 7: Configure Private Route Table

1. Open private-rt
2. Edit routes:

- Destination: 0.0.0.0/0  
- Target: NAT Gateway (my-nat)

---

### Associate Private Subnets

- private-subnet-app  
- private-subnet-db  

---

## 🔐 Step 8: Configure NACL (Network ACL)

1. Go to **Network ACLs**
2. Select default OR create new

---

### Inbound Rules:

| Rule | Type  | Port | Source       | Allow |
|------|------|------|-------------|------|
| 100  | HTTP | 80   | 0.0.0.0/0   | ✔ |
| 110  | HTTPS| 443  | 0.0.0.0/0   | ✔ |
| 120  | Ephemeral | 1024-65535 | 0.0.0.0/0 | ✔ |

---

### Outbound Rules:

Allow all traffic (default)

---

### Associate NACL

- Attach to all subnets

---

## 🔒 Step 9: Security Group Planning (Important Design)

(Not created here but design)

- Web SG → allow HTTP from internet  
- App SG → allow only from Web SG  
- DB SG → allow only from App SG  

---

## 🔄 Final Architecture Flow

Internet → IGW → Public Subnet (Web)  
Private Subnet → NAT → Internet (outbound only)

---

## 🎯 Final Result

✔ Public subnet has internet access  
✔ Private subnets are isolated  
✔ Secure 3-tier network ready  
✔ Proper routing configured  

---

## ⚡ Common Mistakes (VERY IMPORTANT)

❌ Forgetting IGW attach  
❌ No route in public route table  
❌ NAT in wrong subnet  
❌ Not associating route tables  

✔ Always verify routes and subnet associations
