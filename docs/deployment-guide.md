# Deployment Guide

This document explains how to recreate the Highly Available Web Application infrastructure on AWS.

---

# Prerequisites

- AWS Account
- IAM User with Administrator permissions
- AWS Management Console access
- Basic familiarity with EC2, VPC, and Load Balancers

---

# AWS Services Used

- Amazon EC2
- Elastic Load Balancer (Application Load Balancer)
- Auto Scaling Groups
- Amazon CloudWatch
- Amazon SNS
- IAM
- Security Groups

---

# Deployment Steps

## 1. Launch EC2 Instance

Create an EC2 instance using Amazon Linux.

Install Apache:

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl enable httpd
sudo systemctl start httpd
```

Create a simple homepage.

---

## 2. Create an AMI

After validating the server:

- Stop the instance (optional)
- Create an Amazon Machine Image (AMI)

This image will be used by the Auto Scaling Group.

---

## 3. Create Launch Template

Configure:

- AMI
- Instance Type
- Security Group
- Key Pair

---

## 4. Configure Auto Scaling Group

Create an Auto Scaling Group using the Launch Template.

Recommended configuration:

- Minimum Capacity: 2
- Desired Capacity: 2
- Maximum Capacity: 4

Enable Multi-AZ deployment.

---

## 5. Create Application Load Balancer

Configure:

- HTTP Listener
- Target Group
- Register Auto Scaling instances

Verify traffic is distributed correctly.

---

## 6. Configure Health Checks

Health check path:

```
/
```

Healthy threshold:

```
2
```

Unhealthy threshold:

```
2
```

---

## 7. Configure CloudWatch Alarm

Create a CPU utilization alarm.

Example:

- Metric: CPUUtilization
- Threshold: >70%
- Evaluation Period: 5 minutes

---

## 8. Configure SNS Notifications

Create an SNS Topic.

Subscribe an email endpoint.

Attach the CloudWatch Alarm.

Verify notification emails are received.

---

## 9. Validate High Availability

Terminate one EC2 instance manually.

Expected behavior:

- Auto Scaling launches replacement instance
- Load Balancer redirects traffic
- Application remains available

---

# Validation Checklist

- EC2 reachable
- Load Balancer healthy
- Auto Scaling replaces failed instances
- CloudWatch Alarm triggers
- SNS notification delivered

---

# Notes

This infrastructure has since been removed to avoid AWS charges.

The repository preserves:

- Architecture diagrams
- Screenshots
- Documentation

allowing the deployment to be reproduced.

---

# Estimated Cost

AWS Free Tier eligible.

Outside the Free Tier, normal EC2, Load Balancer, and CloudWatch charges apply.

---

# Author

Phillip-Bryan Kouokam

GitHub:
https://github.com/PhilBKouokam