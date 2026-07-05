# CloudWatch Monitoring Lab — Security Monitoring & Alerting

📌 Overview
This project demonstrates how to implement security-focused monitoring and alerting using Amazon CloudWatch, CloudWatch Logs, CloudWatch Alarms, and Amazon SNS. The goal is to detect unauthorized authentication attempts and abnormal NAT gateway outbound traffic, then automatically notify operations teams through SNS email alerts.

This lab simulates a real CloudOps workflow:
- Collect logs
- Convert logs into metrics
- Trigger alarms
- Send notifications
- Validate the alert pipeline end-to-end

Even within the controlled AWS Skill Builder environment, this project showcases real operational skills used by CloudOps engineers.

🏗️ Monitoring Architecture
1. **CloudWatch Logs**
   Collects authentication failure events from the database server.

2. **Metric Filters**
   Converts log patterns (e.g., “authentication failure”) into measurable CloudWatch metrics.

3. **CloudWatch Alarms**
   Triggers when thresholds are exceeded for:
   - Authentication failures
   - NAT gateway outbound traffic

4. **Amazon SNS Topic**
   Receives alarm notifications.

5. **SNS Email Subscription**
   Sends alerts to the operations team.

🔍 What Was Implemented
### 1. Log Monitoring
- Viewed real-time authentication failure logs.
- Identified repeated failed login attempts.
- Created a metric filter to track authentication failures.

### 2. Alarm Configuration
**Authentication Failure Alarm**
- Metric: `database server authentication failures`
- Threshold: > 2 failures within 5 minutes
- Purpose: Detect brute-force or unauthorized login attempts.

**NAT Gateway Outbound Traffic Alarm**
- Metric: `BytesOutToDestination`
- Threshold: > 2,000,000 bytes within 15 minutes
- Purpose: Detect abnormal outbound traffic patterns.

### 3. SNS Notification Pipeline
- Created SNS topic: `CloudWatch_alarm_notifications`
- Added and confirmed email subscription
- Linked alarms to SNS topic
- Verified alarm → SNS → email workflow

🧠 CloudOps Monitoring Workflow
This lab demonstrates the exact workflow CloudOps engineers use to detect and respond to security anomalies:

- Identify suspicious log patterns
- Convert logs into metrics
- Configure alarms with meaningful thresholds
- Route alarms to SNS for notifications
- Validate end-to-end alerting behavior

📸 Screenshots
Screenshots are located in the `/screenshots` folder and include:
- CloudWatch alarms dashboard
- Authentication failure log events
- SNS topic details
- SNS subscription confirmation
- SNS subscription details
- Alarm actions configuration
- Alarm state change history

🧪 Testing & Validation
To validate the monitoring pipeline:
- Triggered authentication failures
- Observed log entries in CloudWatch Logs
- Verified metric filter increments
- Alarm transitioned to “In Alarm” state
- SNS email notification was delivered
- NAT gateway traffic alarm remained in “OK” state (expected)

This confirms the alerting system works end-to-end.

📝 Key Learnings
- How CloudWatch Logs feed into metric filters
- How CloudWatch Alarms detect abnormal patterns
- How SNS integrates with alarms for notifications
- How to validate monitoring pipelines like a CloudOps engineer
- How to document cloud troubleshooting professionally