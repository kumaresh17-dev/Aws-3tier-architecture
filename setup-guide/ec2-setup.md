# EC2 Setup (Web Server & Application Server)

---

## 📌 Overview

In this project, two EC2 instances are created:

- Web Server → Public Subnet (handles incoming traffic)
- Application Server → Private Subnet (handles backend logic)

This separation ensures security and follows 3-tier architecture best practices.

---

## 🧱 Step 1: Launch Web Server (Public Subnet)

1. Go to AWS Console → EC2
2. Click **Launch Instance**

---

### 🔹 Basic Configuration

- Name: web-server
- AMI: Amazon Linux 2023
- Instance type: t2.micro (Free tier)

---

### 🔹 Key Pair

- Create new key pair OR use existing
- Download `.pem` file

---

### 🔹 Network Settings

- VPC: my-vpc  
- Subnet: public-subnet-1  
- Auto-assign Public IP: ENABLED  

---

### 🔹 Security Group (IMPORTANT)

Create new Security Group:

- Allow HTTP (80) → 0.0.0.0/0  
- Allow SSH (22) → Your IP  

---

### 🔹 Storage

- Default (8 GB)

Click **Launch Instance**

---

## 🌐 Step 2: Connect to Web Server

From local machine:

ssh -i your-key.pem ec2-user@<public-ip>


---

## ⚙️ Step 3: Install and Configure Nginx

Run:
sudo yum update -y
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx


---

### Test

- Open browser  
- Enter Public IP  
- Nginx default page should load  

---

## 🔁 Step 4: Configure Reverse Proxy

Edit config:
sudo nano /etc/nginx/nginx.conf


Update:
server {
listen 80;

location / {
    proxy_pass http://<APP_SERVER_PRIVATE_IP>:5000;
}

}


Restart:


sudo systemctl restart nginx


---

## 🧱 Step 5: Launch Application Server (Private Subnet)

1. Launch new EC2 instance

---

### 🔹 Configuration

- Name: app-server  
- AMI: Amazon Linux 2023  
- Instance type: t2.micro  

---

### 🔹 Network

- VPC: my-vpc  
- Subnet: private-subnet-app  
- Auto-assign Public IP: DISABLED  

---

### 🔹 Security Group

Create:

- Allow port 5000 from Web Server Security Group  
- Allow SSH (22) from Bastion or Web Server  

---

Click Launch

---

## 🔐 Step 6: Connect to App Server (IMPORTANT)

Since no public IP:

### Option 1: From Web Server
ssh ec2-user@<app-private-ip>

---

## ⚙️ Step 7: Install Application (Flask Example)
sudo yum install python3 -y
pip3 install flask


---

## 🧪 Step 8: Create Sample App


nano app.py


Paste:


from flask import Flask
app = Flask(name)

@app.route('/')
def home():
return "App Server Running Successfully"

app.run(host='0.0.0.0', port=5000)


Run:


python3 app.py


---

## 🔄 Step 9: Test Full Flow

- Browser → Web Server Public IP  
- Nginx → App Server  
- App response shown  

---

## 🔐 Security Design

- Web Server → Public access  
- App Server → Private only  
- Communication via private IP  
- No direct internet access to App Server  

---

## 🎯 Final Result

✔ Web Server accessible from internet  
✔ App Server isolated in private subnet  
✔ Reverse proxy working  
✔ Full 2-layer communication established  

---

## ⚡ Common Mistakes

❌ App server public IP enabled  
❌ Wrong security group rules  
❌ Nginx not restarted  
❌ Port 5000 blocked  

✔ Always verify connectivity
