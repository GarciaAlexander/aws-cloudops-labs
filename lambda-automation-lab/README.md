# Serverless Architectures using Amazon CloudWatch Events & Scheduled Events with AWS Lambda (Skill Builder Lab)

## 📌 Overview
This project demonstrates how to build event‑driven and scheduled serverless automation using AWS Lambda, Amazon EventBridge (CloudWatch Events), Amazon CloudWatch Alarms, and Amazon SNS.

The AWS Skill Builder lab environment uses a restricted IAM role (AWSLabsUser), which limits access to certain operational features. Even with these constraints, this project showcases real CloudOps operational skills including:

- Event-driven Lambda automation  
- Scheduled Lambda execution  
- CloudWatch alarm configuration  
- SNS operational alerting  
- CloudWatch log analysis  
- Understanding EC2 lifecycle events  
- End‑to‑end serverless workflow validation  

This repository includes:

- EC2 launch event automation  
- Scheduled website checker automation  
- CloudWatch alarm + SNS notification workflow  
- Lambda code (Node.js + Python blueprint)  
- Screenshots demonstrating operational visibility  


## 🏗️ Architecture Components

### **1. Lambda Functions**

#### **MonitorEC2 (Node.js 22.x)**
- Logs EC2 instance state-change events  
- Triggered when an EC2 instance enters the `running` state  

#### **Check-Website-Content (Python Blueprint)**
- Runs every minute  
- Checks a website for expected text  
- Throws an exception on validation failure  


### **2. EventBridge (CloudWatch Events)**

#### **EC2-Launched Rule**
- Event source: `aws.ec2`  
- Detail-type: `EC2 Instance State-change Notification`  
- State filter: `running`  
- Target: `MonitorEC2` Lambda  

#### **Scheduled Event Trigger**
- Schedule expression: `rate(1 minute)`  
- Attached directly to the `Check-Website-Content` Lambda function  
- Executes periodic website checks  


### **3. Amazon EC2**
- Instance name: `TestInstance`  
- AMI: Amazon Linux 2023  
- Instance type: `t3.micro`  
- Public subnet placement  
- Used to validate EC2 → EventBridge → Lambda workflow  


### **4. Amazon SNS**

#### **Topic: WebCheck**
- Type: Standard  
- Used to deliver website failure notifications  

#### **Subscription**
- Protocol: Email  
- Status: Confirmed  
- Receives CloudWatch alarm notifications  


### **5. Amazon CloudWatch**

#### **Logs**
- Lambda execution logs  
- EC2 event JSON  
- Website checker validation output  

#### **Metrics**
- Lambda invocations  
- Lambda errors  
- Alarm state transitions  

#### **Alarm: Website-Failure**
- Metric: Lambda Errors  
- Threshold: ≥ 1 error in 1 minute  
- Action: Send notification to SNS topic `WebCheck`  


## 🔍 Serverless Automation Workflows

### 🧠 **1. EC2 Launch → EventBridge → Lambda**
When an EC2 instance transitions to `running`, EventBridge routes the event to the `MonitorEC2` Lambda function.  
The Lambda logs the full event JSON, including:

- Instance ID  
- Region  
- State  
- Timestamp  
- Account  

This demonstrates event-driven automation in AWS.


### 🧠 **2. Scheduled Website Checker → Lambda → CloudWatch Alarm → SNS**
Every minute, the `Check-Website-Content` Lambda function checks a website for expected text.  
If the validation fails:

1. Lambda throws an exception  
2. CloudWatch records an error metric  
3. CloudWatch Alarm enters ALARM state  
4. SNS sends an email notification  

This demonstrates scheduled automation and operational alerting.


## 📸 Screenshots

### **Part 1 — EC2 Launch Automation**
- MonitorEC2 Lambda code  
- EventBridge EC2-Launched rule  
- EC2 TestInstance running  
- MonitorEC2 Lambda metrics  
- MonitorEC2 CloudWatch logs  

### **Part 2 — Scheduled Website Checker**
- CheckWebsite-Content Lambda (code + environment variables + EventBridge trigger)  
- SNS Topic WebCheck  
- SNS Subscription confirmed  
- CloudWatch Alarm Website-Failure  
- Lambda failure output  
- SNS email notification  

Screenshots are located in the `/screenshots` folder.


## 📝 Summary
This project demonstrates practical CloudOps operational skills:

- Event-driven serverless automation  
- Scheduled Lambda execution  
- CloudWatch alarm configuration  
- SNS operational notifications  
- Lambda log analysis  
- Understanding EC2 lifecycle events  
- End‑to‑end validation of serverless workflows  

This lab reinforces how AWS serverless components work together to monitor infrastructure and respond automatically to cloud events.