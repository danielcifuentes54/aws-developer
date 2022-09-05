<img src="icons/AWS-D.png" />

# AWS Developer Associate
Information about the AWS services that are required in the AWS developer certification.

## Regions

AWS has Regions around the world, a region is a cluster of data centers.

How to choose an AWS Region

- Compliance
- Proximity
- Available Services
- Pricing
___

## Availability Zones (AZ)

Is one or more discrete data centers with redundant power newtworking and connectivity

> Each region has many AZ

---
## IAM (Identity and Access Management)

*  Statement: statement in an IAM Policy consists of Sid, Effect, Principal, Action, Resource, and Condition.
* Users: mapped to a physical user has a password for AWS console.
* Groups: contains users
* Policies: JSON document that outlines for users and groups.
* Roles: for EC2 instances or AWS services.
* Security:
  * Password policy
  * MFA (Multi Factor Authentication)
    * Virtual MFA Device
    * U2F (universal 2nd factor) - security key
    * Hardware Key Fob MFA Device
    * Hardware Key Fob MFA Device for AWS GovCloud (US) 
* Audit: 
  * IAM Credentials Report: List all your account's users
  * IAM Access Advisor : permissions granted and when those services were last accessed
* Ways to access AWS API:
  * Console
  * CLI - Cloud shell
  * SDK
* Cloud shell: is a browser-based shell that makes it easy to securely manage, explore, and interact with your AWS resources. it takes the credentials and permissions from the user who is logged in.

---

## EC2 (Elastic Cloud Computing)

* Instances types
  * General Purpouse.
  * Computed Optimized.
  * Memory Optimized.
  * Storage Optimized.
  * Accelerated computing.

* Name Convention (m5.2xlarge):
  * m --> instance class
  * 5 --> generation
  * 2xlarge -->  size  within the instance class

* security groups: are acting as "firewall" on EC2 instances

* on demand
* Reserved Instances
* Saving plans
* Spot instances
* Dedicated hosts
* Dedicated instances
