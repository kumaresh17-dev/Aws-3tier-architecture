# Application Load Balancer (ALB) Setup – Complete Guide

---

## 📌 Overview

Application Load Balancer (ALB) is used to distribute incoming traffic from CloudFront to Web Servers in the public subnet.

It ensures:
- High availability  
- Fault tolerance  
- Load distribution  

---

## 🎯 Architecture Role

Route53 → CloudFront → ALB → Web Server → App Server → RDS

---

## 🧱 Step 1: Create Target Group

1. Go to AWS Console → EC2  
2. Click **Target Groups**  
3. Click **Create target group**

---

### 🔹 Basic Configuration

- Target type: Instance  
- Target group name: web-tg  
- Protocol: HTTP  
- Port: 80  
- VPC: my-vpc  

Click **Next**

---

### 🔹 Register Targets

- Select Web Server EC2 instance  
- Click **Include as pending below**  

Click **Create target group**

---

## ❤️ Step 2: Configure Health Check (IMPORTANT)

1. Open Target Group → Health checks  

### Settings:

- Protocol: HTTP  
- Path: /  
- Healthy threshold: 2  
- Unhealthy threshold: 2  
- Timeout: 5 sec  
- Interval: 30 sec  

👉 Make sure Nginx is running in Web Server

---

## 🧱 Step 3: Create Application Load Balancer

1. Go to EC2 → Load Balancers  
2. Click **Create Load Balancer**  
3. Select **Application Load Balancer**

---

### 🔹 Basic Configuration

- Name: my-alb  
- Scheme: Internet-facing  
- IP type: IPv4  

---

## 🌐 Step 4: Network Mapping

- VPC: my-vpc  
- Select **2 Public Subnets** (Important for HA)

Example:
- public-subnet-1 (ap-south-1a)  
- public-subnet-2 (ap-south-1b)  

---

## 🔐 Step 5: Security Group

Create ALB Security Group:

- Allow HTTP (80) → 0.0.0.0/0  

👉 (Optional) Allow HTTPS (443)

---

## 🎧 Step 6: Listener Configuration

- Protocol: HTTP  
- Port: 80  

### Default Action:

- Forward to: web-tg (Target Group)

---

## 🚀 Step 7: Create ALB

- Review all settings  
- Click **Create Load Balancer**

⏳ Wait until status = Active

---

## 🔍 Step 8: Get ALB DNS

1. Open ALB  
2. Copy DNS name

Example:
my-alb-123456.ap-south-1.elb.amazonaws.com


---

## 🔐 Step 9: Update Web Server Security Group (VERY IMPORTANT)

Modify Web Server SG:

- Allow HTTP (80) ONLY from ALB Security Group  

👉 NOT from 0.0.0.0/0

---

## 🔄 Step 10: Test ALB

- Paste ALB DNS in browser  
- Verify Nginx page loads  

---

## 🔄 Full Flow

CloudFront → ALB → Web Server → App Server  

---

## 🔐 Security Design

- ALB is public entry point  
- Web Server is protected  
- App Server is private  
- Database fully isolated  

---

## 🎯 Final Result

✔ ALB created successfully  
✔ Traffic distributed to Web Server  
✔ High availability achieved  
✔ Secure architecture maintained  

---

## ⚡ Common Mistakes (VERY IMPORTANT)

❌ Adding App Server in target group  
✔ Only Web Server should be added  

❌ Using private subnet for ALB  
✔ Always use public subnets  

❌ Health check failing  
✔ Ensure Nginx running  

❌ Web Server open to internet  
✔ Restrict to ALB only  

---

## 💡 Pro Tips

- Use HTTPS listener with SSL certificate (ACM)  
- Enable access logs (S3)  
- Integrate with Auto Scaling (future)
