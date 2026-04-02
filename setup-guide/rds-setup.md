# RDS Setup (Database Layer - MySQL)

---

## 📌 Overview

Amazon RDS is used to deploy a managed MySQL database in the private subnet.

In this project:
- RDS is placed in private subnet  
- No public access  
- Only Application Server can connect  

---

## 🎯 Architecture Role

App Server → RDS (Private DB)

---

## 🧱 Step 1: Create DB Subnet Group (VERY IMPORTANT)

1. Go to AWS Console → RDS  
2. Click **Subnet Groups**  
3. Click **Create DB Subnet Group**

---

### 🔹 Configuration

- Name: my-db-subnet-group  
- Description: DB subnet group for project  
- VPC: my-vpc  

---

### 🔹 Add Subnets

Select:

- private-subnet-app  
- private-subnet-db  

👉 Must be in **different AZs** (recommended)

Click Create

---

## 🧱 Step 2: Create RDS Instance

1. Go to **Databases → Create database**

---

### 🔹 Choose Database Creation Method

- Select: Standard Create  

---

### 🔹 Engine Options

- Engine type: MySQL  

---

### 🔹 Templates

- Select: Free tier (or Dev/Test)

---

### 🔹 Settings

- DB instance identifier: my-rds-db  
- Master username: admin  
- Master password: <your-password>

---

### 🔹 Instance Configuration

- DB instance class: db.t3.micro (Free tier)

---

### 🔹 Storage

- Default settings (20 GB)

---

## 🌐 Step 3: Connectivity Configuration

---

### 🔹 Network Settings

- VPC: my-vpc  
- DB Subnet Group: my-db-subnet-group  

---

### 🔹 Public Access

❌ Select: NO (IMPORTANT)

---

### 🔹 VPC Security Group

Create or select:

- Allow MySQL (3306)  
- Source: App Server Security Group  

👉 NOT from 0.0.0.0/0

---

### 🔹 Availability

- Single AZ (for this project)

---

Click **Create Database**

⏳ Wait for status = Available

---

## 🔍 Step 4: Get RDS Endpoint

1. Open DB instance  
2. Copy:

- Endpoint  
- Port (3306)

Example:
my-rds-db.abc123.ap-south-1.rds.amazonaws.com

---

## 🔐 Step 5: Connect from App Server

Login to App Server:
ssh ec2-user@<app-private-ip>
---

## 🧪 Step 6: Install MySQL Client


sudo yum install mysql -y


---

## 🔗 Step 7: Connect to RDS


mysql -h <RDS-ENDPOINT> -u admin -p


Enter password

---

## 🧪 Step 8: Test Database

Inside MySQL:


SHOW DATABASES;

---

## 🔐 Security Design (IMPORTANT)

- RDS in private subnet  
- No public access  
- Only App Server allowed  
- Controlled via Security Groups  

---

## 🔄 Full Flow

User → Web → App → RDS  

---

## 🎯 Final Result

✔ RDS deployed successfully  
✔ Private access only  
✔ App Server connected  
✔ Database secured  

---

## ⚡ Common Mistakes

❌ Public access = YES  
❌ Wrong subnet (public)  
❌ Security group open to all  
❌ App server cannot connect  

✔ Always restrict access properly  

---

## 💡 Pro Tip

For production:

- Use Multi-AZ  
- Enable backups  
- Enable encryption  
