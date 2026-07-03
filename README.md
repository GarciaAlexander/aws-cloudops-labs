# AWS VPC 3‑Tier Web Application Deployment (Skill Builder Lab)

## 📌 Overview
This project demonstrates the deployment of a 3‑tier web application architecture on AWS using a fully isolated VPC.  
The AWS Skill Builder lab environment uses a restricted IAM role (AWSLabsUser), which intentionally blocks access to  
certain operational features such as modifying ALB settings, changing security groups, or enabling ALB access logs  
and VPC Flow Logs.

Even with these constraints, this project showcases real operational skills including:

- ALB health diagnostics  
- CloudWatch monitoring  
- Target group validation  
- Understanding backend port mappings  
- Interpreting response codes and performance metrics  

This repository includes:

- VPC architecture overview  
- ALB + Target Group configuration  
- EC2 backend registration  
- RDS subnet group configuration  
- Monitoring and health check analysis  
- Screenshots demonstrating operational visibility  


## 🏗️ Architecture Components

### **1. VPC**
- Custom VPC (LabVPC)
- CIDR: 10.0.0.0/16

### **2. Subnets**
- Public subnets (for ALB)
- Private subnets (for EC2 + RDS)
- Multi‑AZ design

### **3. Routing**
- Internet Gateway for public subnets  
- NAT Gateway for private subnets  
- Route tables for isolation and controlled egress  

### **4. Security Groups**
- ALB SG (public inbound HTTP/HTTPS)
- EC2 SG (restricted inbound from ALB SG)
- RDS SG (restricted inbound from EC2 SG)

### **5. Load Balancing**
- Application Load Balancer (LabVPCALB)
- Target Group (LabVPCALBTargetGroup)
- Health checks configured on HTTP:80
- EC2 backend registered on port 8443

### **6. Compute**
- EC2 instance (ApplicationServer)
- Private subnet placement
- Backend application listening on port 8443

### **7. Database**
- RDS subnet group created using private subnets
- Multi‑AZ database placement supported


## 🔍 ALB Health Check Diagnostics (CloudOps Troubleshooting)

The Skill Builder lab environment is intentionally locked down and does not allow:

- modifying ALB listeners  
- modifying target group ports or health check paths  
- modifying security groups  
- enabling ALB access logs  
- enabling VPC Flow Logs  

Because of these restrictions, I could not simulate an unhealthy target.  
However, I documented the **exact diagnostic workflow** CloudOps engineers use in real production environments.

This demonstrates operational understanding even without break/fix permissions.


### 🧠 1. Target Group Health Status (Primary Diagnostic Tool)

The Target Group details page shows:

- Healthy hosts  
- Unhealthy hosts  
- Failure reasons  
- Last health check timestamp  
- Availability Zone distribution  

This is the **first place** CloudOps engineers look when an ALB target becomes unhealthy.

In this lab, the target remained healthy, but the screenshot demonstrates where failures would appear.


### 📊 2. ALB Monitoring (CloudWatch Metrics)

The ALB and Target Group Monitoring tabs provide key CloudWatch metrics:

- `HealthyHostCount`  
- `UnHealthyHostCount`  
- `TargetResponseTime`  
- `HTTPCode_Target_4XX_Count`  
- `HTTPCode_Target_5XX_Count`  
- `RequestCount`  

These metrics help identify:

- slow or failing health check responses  
- application errors  
- network latency  
- backend overload  

Even in a locked lab environment, these metrics show how ALB health is monitored in production.


### 📁 3. ALB Access Logs (Production Use Case)

The lab does **not** allow enabling ALB access logs.

In real deployments, ALB access logs stored in S3 provide:

- health check request logs  
- response codes  
- latency  
- routing behavior  
- backend errors  

These logs are essential for diagnosing intermittent health check failures.


### 🌐 4. VPC Flow Logs (Network-Level Diagnostics)

The lab does **not** allow enabling VPC Flow Logs.

In production, flow logs help determine whether ALB health check traffic reaches the EC2 instance.

They reveal:

- allowed traffic  
- denied traffic  
- SG/NACL issues  
- subnet routing problems  

Documenting this step shows understanding of network-level troubleshooting even without access.


### 🧩 5. Common Real-World Root Causes of ALB Unhealthy Targets

These are the issues CloudOps engineers diagnose most often:

- Incorrect health check path (e.g., `/health` vs `/`)  
- Wrong target group port (app listening on 8443 but TG set to 80)  
- Missing inbound SG rule for health check port  
- EC2 instance in wrong subnet (no route to ALB)  
- Application not listening on the expected port  
- App returning 5XX errors during health checks  
- IAM permission issues preventing backend startup  

Even though the lab environment prevented modifying these components, I documented the full diagnostic workflow used in real AWS environments.


## 📸 Screenshots

### **1. Target Group Health Overview**
Shows the registered EC2 instance, port configuration, and health status.

### **2. ALB Health Monitoring (CloudWatch Metrics)**
Displays HealthyHostCount and UnHealthyHostCount over time.

### **3. ALB Performance Metrics**
Includes response time, request count, 2XX/4XX codes, and backend behavior.

Screenshots are located in the `/screenshots` folder.


## 📝 Summary

This project demonstrates practical CloudOps operational skills:

- Understanding ALB → Target Group → EC2 flow  
- Knowing where health check failures appear  
- Knowing which AWS tools to use  
- Knowing how to interpret CloudWatch metrics  
- Knowing how to diagnose common ALB issues  
