# Application Load Balancer Setup (Project-Specific)

---

## 📌 Overview

In this project, the Application Load Balancer (ALB) is used to handle incoming traffic from CloudFront and distribute it across the Web Server layer.

The ALB is deployed in public subnets and acts as an entry point to the application layer, ensuring high availability and fault tolerance.

---

## 🏗️ Architecture Placement

Route 53 → CloudFront → ALB → Web Server (Public Subnet) → App Server (Private Subnet) → RDS

---

## 🎯 Purpose in This Project

- Acts as a bridge between CloudFront and Web Servers  
- Distributes incoming traffic efficiently  
- Improves system availability  
- Enables future scalability (Auto Scaling ready)  

---

## ⚙️ Step-by-Step Setup

### 🔹 Step 1: Create Target Group

- Target type: Instance  
- Protocol: HTTP  
- Port: 80  
- VPC: Select your custom VPC  

👉 Register only **Web Server EC2 instance** (not App Server)

---

### 🔹 Step 2: Create ALB

- Type: Application Load Balancer  
- Scheme: Internet-facing  
- IP type: IPv4  

---

### 🔹 Step 3: Network Configuration

- Select your VPC  
- Choose **2 public subnets** (for high availability)  

👉 Important:
ALB must be in **public subnet**, because it receives internet traffic

---

### 🔹 Step 4: Security Group Configuration

Create ALB Security Group:

- Allow HTTP (Port 80) from anywhere OR CloudFront  
- Outbound: Allow all  

---

### 🔹 Step 5: Listener Configuration

- Protocol: HTTP  
- Port: 80  
- Forward to: Target Group (Web Server)

---

### 🔹 Step 6: Register Targets

- Select Web Server EC2 instance  
- Add to target group  

---

## ❤️ Health Check (Important for this Project)

- Protocol: HTTP  
- Path: `/`  

👉 Ensure:
- Nginx is running in Web Server  
- Port 80 is open  

---

## 🔐 Security Design (Project-Specific)

- ALB is the only public entry point  
- Web Server allows traffic only from ALB  
- App Server is in private subnet (no internet access)  
- Database is completely isolated  

👉 This ensures proper **3-tier security model**

---

## 🔄 Traffic Flow (Real Flow in this Project)

1. User request → Route 53  
2. Route 53 → CloudFront  
3. CloudFront → ALB  
4. ALB → Web Server  
5. Web Server → App Server  
6. App Server → RDS  

---

## 🧠 Design Decision

Why ALB is used:

- Layer 7 routing (HTTP/HTTPS)  
- Better integration with CloudFront  
- Supports scaling in future  
- Provides health checks  

---

## 🔥 Testing

- Copy ALB DNS  
- Access in browser  
- Verify Nginx response  

---

## ⚡ Common Mistakes

❌ Adding App Server in target group  
✔ Only Web Server should be added  

❌ Keeping Web Server open to internet  
✔ Restrict access only from ALB  

❌ Wrong subnet selection  
✔ Always use public subnets for ALB  

---

## 🎯 Final Result

- Traffic successfully routed via ALB  
- High availability achieved  
- Proper 3-tier architecture maintained  
