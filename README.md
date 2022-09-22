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

* EC2 instances Purchasing Options
  * on demand.
  * Reserved Instances.
  * Saving plans.
  * Spot instances.
  * Dedicated hosts.
  * Dedicated instances.

## EBS (Elastic Block Store)

* An EBS volume is a network drive you can attach to your instances, it uses the network to communicate the instance, it's locked to an AZ and have a provisioned   capacity (size in GBs, and IOPS).
* You can make a backup (snapshot) of your EBS volume at a point in time (can copy snapshots accross AZ or region).
  * EBS Snapshot Archive.
  * Recycle Bin for EBS Snapshots: Protect your EBS Snapshots.
* Types:
  * SSD
    * gp2/gp3 (SSD):
      * cost-effective storage latency
      * In gp3 you can independently set the IOPS and the throughput, in gp2 they're linked
      * Max IOPS 16000 IOPS
    * io1 / io2 (SSD)
      * For applications that need more than 16000 IOPS
      * Allows Multi-Attach
  * HDD (cannot be a boot volume)
    * st1 (HDD)
      * Big Data, Data Warehouses, Log processing
    * sc1 (HDD)
      * For data that is infrequently accesed



## AMI (Amazon Machine Image)

* AMI are a customization of an EC2 instance.
* an AMI creates also an EBS snapshot.

## EC2 Instance Store

* It is an hard drive on the EC2 instance
* Better I/O performance
* Ephemeral Storage

## Amazon EFS (Elastic File System)

* Use security group to contrl access

## High Availability & Scalability 

* Vertical Scaling (scale up / down)
* Horizontal Scaling (scale out / in)
* High Availability
  * load balancer (active)
  * multi az (passive)

## AWS ELB (Elastic Load Balancer)
* types:
  * Classic load balancer (Layer 4 - layer 5 / v1 old generation)
  * Application load balancer (Layer 7 / v2 new generation)
    * support redirects (from http to https for example)
    * Allow routing tables to different target groups
    * are great fit for microservices 
    * alb can route to multiple target groups
  * Network load balancer (Layer 4 (TCP/UDP) / v2 new generation)
    * Allows one static IP per AZ
    * Handle millions of request per second
    * Are used for extreme performance, TCP or UDP traffic
  * Gateway load balancer (layer 3 - network)
    * Uses the GENEVE protocol on port 6081
    * Inspect all the network traffic before sent to the application

* Sticky session: 
  * The client is always redirected to the same instance behind a load balancer
  * ELB and CLB
  * cookies:
    * Application-based cookies
    * Duration-based cookies