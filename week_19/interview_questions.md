# Week 19 — Interview Questions: AWS Core for AI Engineers

> Sourced from cloud/FDE/infra screens and AWS certification banks (SAA-level, applied).

---

## Fundamentals & IAM
1. What is the shared responsibility model?
2. Regions vs availability zones vs edge locations.
3. **How does IAM enforce least privilege?** Users vs groups vs roles vs policies.
4. **How would you give an EC2 instance access to S3 without hardcoding credentials?**
5. Access keys vs IAM roles — why are roles safer?

## Compute — EC2 & Auto Scaling
6. EC2 instance families and AMIs — how do you choose an instance type?
7. **What is the difference between a security group and a network ACL?**
8. How do Auto Scaling Groups and scaling policies work?
9. How do you start/stop/terminate and check EC2 status via boto3?

## Storage
10. S3 storage classes, versioning, and lifecycle policies — when to use each.
11. EBS vs EFS vs S3 — when to use each for AI workloads?
12. What is an EBS snapshot and how does recovery work?

## Networking
13. Walk through a VPC: public/private subnets, route tables, internet gateway, NAT gateway.
14. **Walk me through what happens when a request hits your ALB.**
15. ALB vs NLB — when to use each.
16. What does Route 53 do? What does ACM provide?
17. What is VPC peering?

## Security & Observability
18. What is KMS and when do you encrypt with it?
19. Secrets Manager vs SSM Parameter Store.
20. What do CloudWatch and CloudTrail each give you?

## System Design
21. Provision a production VPC + EC2 behind an ALB with an ASG — describe the architecture.

## Failure / Judgment
22. A public S3 bucket leaked PII — misconfigured policy, no encryption, no CloudTrail. Fix it.
23. "IAM role or access keys for this service-to-service call?" — defend it.
