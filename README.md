
# Cloud-Support-Lab

Hands-on AWS troubleshooting projects built in a self-managed Free Tier environment, documented as support tickets: problem, diagnosis, root cause, fix.

## Projects

### 1. [IAM & S3 Security Lab](./iam-s3-security)
Identity, access management, and secure storage.
**Services:** IAM, S3, KMS, MFA, Bucket Policies
**Covers:** permission errors, misconfigured policies, public bucket exposure, versioning, encryption, least privilege, MFA

### 2. [EC2 Deployment & Network Troubleshooting](./ec2-network-troubleshooting)
Compute, networking, and connectivity.
**Services:** EC2, VPC, Security Groups, NACL, Route Tables, Internet Gateway, EBS, Elastic IP
**Covers:** Linux EC2 deployment, SSH failures, security group/route table misconfigurations, NACL blocks, EBS attachment, public vs private subnets

### 3. [Monitoring, Logging & Incident Response](./monitoring-incident-response)
Observability and operational support.
**Services:** CloudWatch, CloudWatch Logs & Alarms, CloudTrail, SNS
**Covers:** CPU/disk alarms, status check failures, log analysis, alert notifications, dashboards, incident investigation

### 4. [End-to-End Support Ticket Simulation](./support-ticket-simulation)
Real-world support scenarios spanning multiple services — the capstone project.
10–15 tickets including: EC2 SSH failure, S3 access denied, site down, high CPU alert, IAM lockout — each diagnosed and resolved across the relevant services from Projects 1–3.

## Approach

Each project sets up a real AWS environment, deliberately introduces a fault, then documents the diagnostic process and fix — the same structure used for an actual support ticket. The final project applies all three skill areas to simulate a support engineer's actual ticket queue.

## Stack

AWS (IAM, EC2, VPC, S3, CloudWatch, CloudTrail, SNS, KMS), AWS CLI, IAM Policy Simulator
