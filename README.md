# 🚀 High Availability (HA) Architecture on AWS – Step by Step

This repository showcases a **High Availability (HA) setup on AWS**, designed for **scalability, fault tolerance, and reliability**. Perfect for learning cloud architecture, auto scaling, and monitoring.

---

## 1️⃣ VPC & Subnets
- **VPC Name:** HA-VPC (CIDR: 10.0.0.0/16)  
- **Public Subnets:**  
  - Public-Subnet-A → 10.0.1.0/24 (AZ: ap-south-1a)  
  - Public-Subnet-B → 10.0.2.0/24 (AZ: ap-south-1b)  
- **Private Subnets:**  
  - Private-Subnet-A → 10.0.3.0/24 (AZ: ap-south-1a)  
  - Private-Subnet-B → 10.0.4.0/24 (AZ: ap-south-1b)  

---

## 2️⃣ Internet & NAT Gateways
- **IGW:** HA-IGW → Internet access for public subnets  
- **NAT Gateway:** HA-NAT-A in Public-Subnet-A (optional HA-NAT-B for redundancy) → private instances get internet securely  

---

## 3️⃣ Route Tables
- **Public-RT:** 0.0.0.0/0 → HA-IGW  
- **Private-RT-A/B:** 0.0.0.0/0 → HA-NAT-A/B  

---

## 4️⃣ Security Groups & IAM
- **ALB-SG:** HTTP/HTTPS from anywhere  
- **EC2-SG:** HTTP from ALB only  
- **RDS-SG:** MySQL (3306) from EC2 only  
- **IAM Role (EC2-SSM-Role):** AmazonSSMManagedInstanceCore, CloudWatchAgentServerPolicy  

---

## 5️⃣ Launch Template & Auto Scaling
- **Launch Template:** HA-WebServer-LT → Amazon Linux 2023, t3.micro, Apache + PHP, EC2-SG  
- **Auto Scaling Group (HA-ASG):** Private Subnets, Desired=2, Min=2, Max=4  
- **Scaling Policy:** CPU >70% → scale out, CPU <30% → scale in  

---

## 6️⃣ ALB & Target Group
- **ALB:** HA-ALB → Internet-facing, Public Subnets, Listener HTTP 80 → HA-TG  
- **Target Group:** HA-TG → Health Check `/index.php`  

---

## 7️⃣ RDS Database
- **Name:** HA-RDS → MySQL 8.0, Private Subnets, Multi-AZ optional, Public Access NO  
- **Endpoint example:** `ha-rds.abcxyz.ap-south-1.rds.amazonaws.com`  

---

## 8️⃣ CloudWatch & SNS
- **Alarms:** CPU-High → ASG scale out, CPU-Low → ASG scale in, ALB 5xx errors → notify via SNS  
- **SNS:** Email notifications for real-time monitoring  

---

## 9️⃣ Testing
1. Open ALB DNS → `/index.php` → EC2 instance IDs visible  
2. Run stress test → ASG launches new instance (scale out)  
3. Stop stress → CPU <30% → ASG scales in  
4. `/dbtest.php` → DB connectivity successful  

---

## ✅ Key Takeaways
- Subnets across AZs ensure true HA  
- ALB + ASG → load balancing & fault tolerance  
- CloudWatch + SNS → proactive monitoring & alerting  
- Private RDS → secure, highly available database  

---

This setup is **production-ready** and ideal for **learning AWS cloud architecture, scaling, and monitoring**.

---
🔗 LinkedIn Post Check out my LinkedIn post about this project 👉 [https://www.linkedin.com/posts/md-faizan-bb9998234_aws-docker-flask-activity-7367501682735300608-5CgB?utm_source=share&utm_medium=member_desktop&rcm=ACoAADqFwb0B3mY-3tfXzS8MUobP_65azcolSNo](https://www.linkedin.com/posts/md-faizan-bb9998234_aws-cloudcomputing-highavailability-activity-7367837287914655746-Tqw6?utm_source=share&utm_medium=member_desktop&rcm=ACoAADqFwb0B3mY-3tfXzS8MUobP_65azcolSNo)
---
#AWS #CloudComputing #HighAvailability #AutoScaling #DevOps #LearningByDoing
