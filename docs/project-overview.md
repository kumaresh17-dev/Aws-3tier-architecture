## 📌 Project Overview

This project demonstrates the design and implementation of a secure, scalable, and production-ready 3-tier architecture on AWS.

The system is structured into three layers:

* Presentation Layer using Amazon CloudFront and Amazon S3 for efficient content delivery
* Application Layer using Nginx on EC2 instances and an Application Load Balancer for handling incoming traffic
* Database Layer using Amazon RDS deployed in private subnets for secure data storage

The architecture is built using a custom VPC with public and private subnets, ensuring proper network isolation and controlled access between layers.

Key AWS services such as CloudFront, S3, EC2, ALB, and RDS are integrated to simulate a real-world cloud deployment model.

The project focuses on implementing security best practices, high availability, and efficient request flow while maintaining a clear separation of concerns between different components.

This setup reflects a real-world production environment and demonstrates practical understanding of cloud infrastructure design.
