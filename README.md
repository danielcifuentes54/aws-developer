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
  * m :arrow_right: instance class
  * 5 :arrow_right: generation
  * 2xlarge :arrow_right:  size  within the instance class

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

* Cross Zone Load Balancing: 
  * Enabled by default - ALB
  * Disabled by default - CLB / NLB (you have to pay if it's enable)
  * Each load balancer instance distributes evenly across all registred instances all AZ

* SSL
  * Server Name Identification: solve the problem of loading multiple SSL certificates on one web server

* Connection Draining CLB (Deregistration Delay ALB & NLB)
  * Time to complete "in-flight requests" while the instance is de-registering or unhealthy
  * The users that are already connected to that EC2 instance are going to be given enough time, which is the draining period

## Auto Scaling group
* The goal is increased (scale out) and decreased load (scale in).
* It is possible to scale an ASG based on cloudwatch alarms.
* Scaling Policies
  * Dynamic Scaling
    * Simple / Step scaling (ex: cloudwatch alarm)
    * Target tracking scaling (ex: ASG CPU to stay)
    * Scheduled actions (crontab)
  * Predictive Scaling
    * continuously forecast load and schedule scaling ahead
* Scaling Cooldowns
  * after a scaling activity happens you are in the cooldown period and the ASG will not launch or terminate additional instances
* Min size / Max size / Initial Capacity

 ## Amazon RDS

* It is a DB managed service 
* databases:
  * postgres
  * MySql
  * MariaDB
  * Oracle
  * Microsoft SQL Server
  * Aurora 
* Backups
  * Automated backups
  * Snapshots
* Transaction logs 
* RDS Storage Auto Scaling
  * Help you increase storage on your RDS DB instance dynamically

### Read Replica

* Help scale your reads
* up to 5 Read replicas
* Within AZ, Cross AZ, or Cross Region 
* Replication is ASYNC so reads are eventually consistent
* Can be promoted to their own DB
* Application must update the connection string to leverage read replicas
* network cost:
  * in the same region you don't pay that fee

### Multi AZ

* used for disaster recovery
* Sync
* one DNS name - automatic failover
* increase availability
* No manual intervention in apps
* Not used for scaling
* The read replica be setup as Multi AZ for Disaster Recovery (DR)
* from single-az to multi-az
  * Zero downtime operation (no need to stop the DB)
  * Just click on "modify" for the database.

### Amazon Aurora

* Technology from AWS
* Postgres and MySQL are both supported
* 5X performance over MySQL and 3X over postgress
* Storage increments up to 128 TB
* Can have 15 replicas while MySQL 5 
* sub 10 ms replica lag
* it's HA Native 
* Cost 20% more
* 6 copies of your data across 3 AZ
* read replicas can be become the master
* Cluster
  * Writer Endpoint (master)
  * Reader Endpoint (Read Replica)
* Shared storage volume auto expand
* Backtrack: Restore data at any point of time without using backups

## Amazon ElastiCache 

* Managed cache Technologies (in-memory databases Redis or Memcached)
* AWS takes care of OS maintenance / patching optimization, setup, configuration, monitoring, failure recovery and backups
* Cache must have an invalidation strategy to make sure only the most current data is used in there.
* Cache is use also for User Session Store

### Redis

* Multi AZ with failover
* Read replicas to scale reads and have high availability
* Data durability using AOF
* Backup and restore features

### Memcached

* Multi node for partition of data (sharding)
* No HA
* Non persistent
* No backup and restore
* Multi-thread architecture

### Elastic Cache Strategies

* keep in mind :arrow_right: use cache when data changing slowly and few keys
* cache hit: find the key in the cache
* cache miss: key not found in the cache 
* Design patterns:
  * Lazy loading / Cache-aside / Lazy population:
    * In a cache miss: read the data from the DB :arrow_right: write to the cache
    * :white_check_mark: Only requested data is cached.
    * :white_check_mark: Node failures are not fatal.
    * :x: Delay for the request - 3 round trips (in cache miss case).
    * :x: Data can be updated in the database and outdated in the cache.
  * Write Through:
    * Add or update cache when database is updated: insert/update DB  :arrow_right: write to the cache.
    * :white_check_mark: Data in cache is never stale, reads are quick.
    * :white_check_mark: Write penalty (require 2 calls) vs Read penalty (UX - an user expecting more a write longer than a read).
    * :x: Missing data until it is added (you can combine the lazy strategy as well)
    * :x: a lot of data will be never read.
* Cache evictions and time-to-live (TTL)
* evictions can occur in three ways:
  * Delete item explicitly 
  * Item is evicted because the memory is full and it's not recently used (LRU)
  * You set an item time-to-live (TTL)
    * TTL can range from few seconds to hours or days
    * It's not a good idea when you're using **Write through**
* Replication:
  * Cluster mode **disabled**:
    * One shard, all nodes have all the data
    * Helpful to scale read performance  
  * Cluster mode **enabled**:
    * Data is partitioned across shards (helpful to scale writes)

## Route53

* Is also a domain register
* Ability to check the health of your resources
* 100% availability SLA
* If you buy your domain in a 3rd party registrar, you can still use Route 53 as the DNS Service provider
* Record types:
  * A :arrow_right: IPv4
  * AAAA :arrow_right: IPv6
  * CNAME :arrow_right: hostname - Only for non root domain
  * NS :arrow_right: Name servers
* Hosted Zone 
  * A container for records that define how to route traffic to a domain and its subdomain 
  * public and private
* Commands:
  * > nslookup `example.com`
  * > dig `example.com`
* TTL 
  * Is mandatory for each DNS record, except for alias recrods
* Alias
  * Allows root domain
  * native healthcheck
* Healthcheck:
  * Only for public resources
  * Automated DNS Failover
  * Allows combine the results of multiple health checks into a single health check
* routing policy:
  * simple: 
    * Single resource and multiple resources (The client recive all the resources). 
    * It can't be associated with healthchecks.
    * 
  * weighted: 
    * Assign each record a relative weigh.
    * It can be associated with healthchecks.
    * Use case: Load balancing between regions
  * Latency:
    * redirect the resource that has the least latency close to us.
  * failover:
    * Allows to asign a primary and a secondary resource.
  * Geolocation:
    * Specify location by continent, country or by US state.
  * Geoproximity:
    * Is really helpful when you need the traffic from one region to another, by increasing the bias.
    * Traffic Policy:
      *  Simplify the process of creating and maintaining records in large and complex configurations
  * Multi-value
    * Return multiple values/resources
    * can be associated with health checks
    * It is not a substitute for ELB

## VPC

* virtual private cloud (regional resource)
* subnet: allow partition your network inside your vpc (AZ resource), it could be private or public.
* Internet Gateways: helps our VPC instances connect with the internet.
* NAT Gateways: allow your intances in your private subnets to access the internet while remaining private.
* Netowrk ACL:  A firewalll which controls traffic from and to subnet / Allow and deny rules.
* Security Groups: A firewall that controls traffic to and from an ENI (elastic network interface) / an EC2 instance.
* VPC peering: Connect two VPC, privatily using AWS' network
  * Must no have overlapping CIDR 
  * non transitive
* VPC flow logs: network traffic logs.
* VPC endpoints: Allow you to connect to AWS services using a private network (S3 / DynamoDB)
* VPC endpoint interface (ENI): Allow you to connect to AWS services using a private network (Other services).
* Site to site VPN: connect an on-premises VPN to AWS
  * goes over the public internet
* Direct connect (DX): Establish a physical connection between on-premises and AWS
  * goes over the private internet
    
## Serverless

* Initially... Serverless == FaaS (Function as a Service)
* Serverless does not mean there are no serverless, it means you just don't manage / provision / see them
* Serverless in AWS:
  * AWS lambda.
  * DynamoDB.
  * S3.
  * AWS Cognito.
  * AWS API gateway.
  * Amazon S3.
  * AWS SNS & SQS.
  * AWS Kinesis Data firehouse
  * Aurora serverless.
  * Step Functions.
  * Fargate.

## AWS Lambda

* Virtual funtions - no servers to manage.
* Limited by time - short executions.
* Run on-demand.
* Scaling is automated.
* Benefits:
  * Pay per request and compute time.
    * $0,20 per 1 million requests
    * $1,00 for 600000 GB Seconds
  * Integrated with a whole AWS suit services, for example:
    * API gateway.
    * Kinesis.
    * DynamoDB.
    * S3.
    * Cloudfront.
    * Cloudwatch events, eventbridge.
    * Cloudwatch logs.
    * SNS.
    * SQS.
    * Cognito.
  * Integrated with many programming languages:
    * Node Js
    * Java
    * C# (.NET core)
    * Golang
    * C# / powershell
    * Ruby
    * Custom runtime API (Community supported, ex Rust)
    * Lambda container image.
      * The container must implement the Lambda runtime API.
      * ECS / Fargate is prefered for running arbitrary Docker images.
  * Easy monitoring (AWS cloudwatch).
  * Easy to get more resources (up to 10gb RAM), increasing RAM will also improve CPU and network.
  * Resource based policy
    * It Permissions to other AWS accounts or services.
* Synchronous invocations
  * Results is returned right away.
  * Error handling must happen client side
  * Some services:
    * ALB, Api gateway, Cloudfront (Lambda@Edge), S3 Batch, cognito, step functions, Lex, Alexa, Kinesis Data Firehouse.
* Asynchronous invocations
  * you don't need to wait for the result
  * Some services: S3, SNS, Cloudwatch events / event bridge, codecommit, codepipeline
  * you can configurate a DLQ in the Lambda.
* Event Source Mapping
  * Records need to be polled from the source.
  * Lambda is invoked synchronously.
  * Services:
    * Streams:
      * Kinesis Data Streams.
      * DynamoDB streams.
    * Queue
      * SQS & SQS FIFO queue

### Lambda integration with ALB

* The lambda function must be registered in the target group 
* Conversions:
  * Alb :arrow_right: Lambda: Http :arrow_right: Json.
  * Lambda :arrow_right: ALB: Json :arrow_right: HTTP.
* ALB Multi-Header values
  * It is an target group attribute, HTTP headers and query string parameters that are sent wiht multiple values are shown as arrays within the Lambda event and response objects.
  * > `http://example.com/path?name=foo&name=bar`
  * > `"queryStringParameters": {"name":["foo","bar"]}`
*  Resource based policy
  * Allows our target group to invoke our lambda function

### Lambda@Edge

* You can use lambda to change cloudfront requests and responses:
  * Viewer Request.
  * Origin Request.
  * Origin Response.
* Some Use cases:
  * Bot mitigation at the edge.
  * Real time image transformation.
  * User Authentication and Authorization.

### Lambda - Eventbridge

* You can configure lambda to be executed by a cloudwatch event or an schedule.

### Lmabda - S3

* Allows us execute a Lambda when an action is made in an configurated S3 bucket.
  