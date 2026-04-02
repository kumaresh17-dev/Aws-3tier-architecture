# 🚀 Production-Grade AWS 3-Tier Architecture with High Availability and Security

---

## 📌 Introduction

This repository presents the design and implementation of a production-grade AWS 3-tier architecture used to host scalable and secure web applications.

The architecture follows industry-standard best practices by separating the system into independent layers, ensuring improved scalability, fault tolerance, and security.

---

## 📌 Project Overview

This project demonstrates the design and implementation of a secure, scalable, and production-ready 3-tier architecture on AWS.

The system is structured into three layers:

* Presentation Layer using Amazon CloudFront and Amazon S3 for efficient content delivery
* Application Layer using Nginx on EC2 instances and an Application Load Balancer for handling incoming traffic
* Database Layer using Amazon RDS deployed in private subnets for secure data storage

The architecture is built using a custom VPC with public and private subnets, ensuring proper network isolation and controlled access between layers.

Key AWS services such as CloudFront, S3, EC2, ALB, and RDS are integrated to simulate a real-world cloud deployment model.

The application is accessed using the CloudFront distribution URL without custom domain configuration.

---

## 🎯 Objective

* Architect a scalable cloud-native infrastructure
* Implement secure network isolation
* Design a high-availability system
* Optimize request handling and content delivery
* Follow AWS best practices for production environments

---

## 🏢 System Design Approach

The architecture was designed based on real-world system design principles:

* Separation of concerns across layers
* Network isolation using public and private subnets
* High availability using load balancing
* Fault tolerance by avoiding single points of failure
* Optimized content delivery using CDN

Each layer is independently scalable and protected from direct external exposure.

---

## 🏗️ Architecture Overview

### 🔹 Presentation Layer

* Amazon S3 for static asset storage
* Amazon CloudFront for global CDN distribution

---

### 🔹 Application Layer

* Nginx Web Server deployed in public subnet
* Application Server deployed in private subnet
* Application Load Balancer distributes incoming traffic

---

### 🔹 Database Layer

* Amazon RDS (MySQL) deployed in private subnet
* No public accessibility
* Controlled access only from application layer

---

## 🌐 Network Architecture

* Custom VPC (10.0.0.0/16)
* Public subnet for web layer
* Private subnets for application and database layers
* Internet Gateway for inbound traffic
* NAT Gateway for outbound traffic from private subnet
* Route tables configured for traffic flow
* Network ACLs for additional security

---

## 🔄 Request Flow

1. User accesses application via CloudFront URL
2. Request is routed to Amazon CloudFront
3. CloudFront serves cached content or forwards request
4. Request reaches Application Load Balancer
5. Load Balancer distributes traffic to Web Server
6. Web Server forwards request to Application Server
7. Application Server interacts with Amazon RDS
8. Response is returned to the user

---

## ⚡ Engineering Decisions

* Implemented private subnets for application and database layers to reduce attack surface
* Used Application Load Balancer for traffic distribution and high availability
* Integrated CloudFront to improve latency and global performance
* Designed VPC with proper routing and subnet isolation
* Ensured database is not publicly accessible

---

## 🔐 Security Implementation

* Network isolation using public and private subnets
* Security groups to control inbound and outbound traffic
* Network ACLs for subnet-level protection
* No direct internet access to database
* NAT Gateway for secure outbound traffic
* CloudFront as an additional security layer

---

## 📸 Screenshots

All infrastructure screenshots are available in the `/screenshots` directory.

Includes:

* VPC Configuration
* Subnets
* Route Tables
* Internet Gateway
* Network ACLs
* EC2 Instances
* RDS Database
* Load Balancer
* CloudFront Distribution

---

## ⚙️ Implementation Steps

1. Designed and created custom VPC
2. Configured subnets and routing
3. Deployed EC2 instances for web and application layers
4. Configured Nginx as a reverse proxy
5. Set up Amazon RDS database
6. Configured Application Load Balancer
7. Integrated CloudFront with S3 and ALB

---

## 🧠 Additional AWS Knowledge

In addition to the core implementation, I have hands-on practice and understanding of the following AWS services and how they can be integrated into this architecture:

* Auto Scaling for dynamic scaling of EC2 instances
* CloudWatch for monitoring, metrics, and alerting
* Elastic Load Balancer (ELB) concepts
* Elastic Network Interface (ENI) for advanced networking
* DynamoDB for NoSQL use cases
* Route53 for domain based routing

These services were explored separately and can be incorporated to further enhance the system.

---

## 🎯 Key Highlights

* Production-grade architecture
* High availability and scalability
* Secure infrastructure design
* Strong networking implementation
* Real-world deployment model

---

## 🔮 Future Enhancements

* Auto Scaling Group implementation
* CloudWatch monitoring and alerting
* CI/CD pipeline integration
* Infrastructure as Code (Terraform)
* Containerization using Docker and Kubernetes
* Route53 domain based
---

## 📌 Conclusion

This architecture reflects a real-world cloud deployment model and can be extended further for enterprise-level applications.

---

## 👨‍💻 Author

Kumaresan K
