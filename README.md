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

* Statement: statement in an IAM Policy consists of Sid, Effect, Principal, Action, Resource, and Condition.
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
* Check Polices Permissions:
  * Policy Simulator
  * --dry-run
  * aws sts decode-authorization-message: decode the authorization messages errors.

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

* EC2 Instance Metadata: get metadata from an EC2 instance
  * `curl http://169.254.169.254/latest/meta-data`

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

## S3

* use for backup storage, disaster recovery, archive, application hosting, media hosting, data lakes, static website
* buckets have a globally unique name and are define at the region level!.
* you can see all the buckets acrross the regions but each bucket is asign to one region.
* Objects (files): have a key composed by the full path
  * Max object size 5TB
  * if upload more than 5GB use multipart
* Security:
  * User based (IAM Policies)
  * Resource based 
    * Bucket policies (allow cross account):
      * JSON based policies
    * Object access control list (ACL)
    * Bucket access control list (ACL)
  * Encryption: encrypt objects.
  * S3 MFA Delete
  * S3 Access Logs
  * S3 Presigned URLs
  * S3 access points
* Versioning:
  * It is enabled at bucket level.
  * Same key overwrite will change the "version"
  * Protect against unintended deletes (ability to restore)
* Replication:
  * Must enable versioning in source and destination buckets
  * Cross-Region replication (CRR): for compliance, lower latency, replication across accounts
  * Same-Region replication (SRR): for log aggreataion, live replication between product and test accounts
  * Copy is asynchronous.
  * S3 batch replication (To copy existing objects)
  * There's no a chaining replication 
* Storage Classes:
  * Amazon S3 standard - General purpose
  * Amazon S3 Standard-Infrequent Access (IA)
  * Amazon S3 one-zone Infrequent Access 
  * Amazon S3 Glacier Instant Retrieval
  * Amazon S3 Glacier Flexible Retrieval
  * Amazon S3 Glacier Deep Archive.
  * Amazon S3 Intelligent Tiering.
* Durability (99,99999999999%) and  Availability (99,99%).

### S3 Lifecycle Rules

* Transition Actions: Transition to another storage class
* Expirations Actions: configure objects to expire (delete) after some time.
* S3 Analytics - Storage class analytics: Help you decide when to transition objects to the right storage class

### S3 Event Notifications

* you can create as many "s3 events" as desired, this notifications react to events in S3 like S3:ObjectCreated, S3:ObjectRemoved... this events could be send to SQS, SNS, Lambda and event bridge.

### S3 Baseline Performance

* Automatically scales to high requests, latency 100-200 ms
* 3500 PUT/COPY/POST/DELETE and 5500 GET/HEAD requests per second per prefix in a bucket
* performance best practices:
  * Multi-part upload.
  * S3 transfer acceleration: Trasnfering file to an AWS Edge Location.
  * S3 byte-range fetches: Parallelize GETS.

### S3 Select & Glacier Select

* Retrieve less data using SQL by performing server-side filtering
* Can filter by rows & columns (Simple SQL statements).

### S3 - Object Encryption

* Server Side Encryption (SSE)
 * SSE-S3 (Managed by AWS)
 * SSE-KMS
  * :white_check_mark: Audit key usage CloudTrail
  * :x: When you download, it calls the Decrypt KMS API (Quotas)
 * SSE-C (Managed by Customer)
* Client-Side Encryption

### S3 CORS (Cross-Origin Resource Sharing)

* Web browser based mechanism to allow requests to other origins while visiting the main origin.

## AWS SDK

* perform actions on AWS directly from your applications code
* AWS CLI uses the python SDK

## AWS Limits (Quotas)

* API rate limits
  * Exponential Backoff (ThrottlingException)
    * Retry mechanism already included in AWS SDK  API calls
    * Custom retries, each retry should take twice as long
* Service Quotas

## AWS CLI credentials provider chain

* The CLI will look for credentials in this order (precedence):
  1. Command line options.
  2. Environment variables.
  3. CLI credentials file.
  4. CLI configuration file.
  5. Container credentials.
  6. Instance profile credentials.
* Use IAM Roles as much as posible.

## AWS signature v4 signing

* AWS protocol
* API calls needs to be signed with your credentials and the process is called SigV4

## AWS Cloudfront

* Content delivery network (CDN), it has DDoS protection and improves read performance, content is cached at the edge.
* Origins: S3 buckets, Custom origin (HTTP).
* Caching & Invalidations: 
  * Cache based on: headers, session, Query String paramters
  * Maximize cache hits by separating static and dynamic distributions.
  * Control the TTL (0 seconds to 1 year)
  * You can invalidate part of the cache using the CreateInvalidation API
* Security: 
  * Geo Restriction.
  * HTTPS: Viewer and Origin protocol Policy
  * CLoudFront Signed URL / Signed cookies:
    * You should create an application that use the SDK in order to generate the signed URL/Cookie
    * CLoudFront Signed URL: 1 file per signed URL
    * CloudFront Signed cookies: multiple files per signed cookie
    * Signers:
      * (Recommended) Trusted Key groups
      * cloud frint key pair (need the root account)
  * Te cost of the data out per edge location varies.
  * Price classes:
    * Price Class All.
    * Price Class 200.
    * Price Class 100.
  * Multiple Origin: To route to different kind of origins based on the content type.
  * Origins groups: To increase high-availability and do failover

## ECS (Elastic Container Service)

* Launch Types:
  * EC2 Launch type: 
    * you must provision & maintain the infrastructure (EC2 instances)
    * Each EC2 instance must the run the ECS agent 
  * Autoscaling group scaling:
    * ECS service average CPU
    * ECS service average Memory
    * ALB Request count
    * Target tracking 
    * Step Scaling
    * Scheduled Scaling
  * ECS Cluster Capacity provider
  * Fargate Launch Type:
    * You do not provision the infrastructure 
  * AWS just runs ECS tasks based on the CPU and memory
* IAM Roles for ECS:
  * EC2 instance profile.
  * ECS task role.
* Data volumes: Mount EFS file systems onto ECS tasks
* Rolling updates: We can control how many tasks can be started and stopped, and in which order.
* ECS task definition: 
  * Metadata in Json form to tell ECS how to run a docker container
  * Can define up to 10 containers per task
  * One IAM Role Per task definition
  * Data volumens (Bind mounts): Shared data between multiple containers in the same task definition (sidecars containers)
  * Instance task definition
    * image name
    * port binding
    * Memory and CPU required
    * Environment variables:
      * Hardcoded
      * SSM parameter store
      * Secret manager
      * Environment files (Bulk) - S3
    * Network information
    * ECS task placement
      * strategies:
        * Binpack
        * Random
        * Spread
    * IAM role
    * Loggin configuration
  * Fargate task definition
    * Each task has a unique private ip
    * only define the container port

## Elastic Beanstalk

* Elastic Beanstalk is a developer centric view of deploying an application on AWS.
* Components:
  * Application
  * Application version
  * Environment
* Architecture:
  * Single instance: great for dev
  * High Availability with ELB: great for prod.
* [Deployments modes](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html):
  * All At Once:  deploy all in one go (Downtime)
  * Rolling: Application is running below capacity
  * Rolling with additional batches: Application is running at capacity
  * Inmutable: New code is deployed to new instances on temporary ASG
  * Traffic Splitting: Canary Testing
* CLI: We can install an additional CLI called the "EB cli" which makes working with Beanstalk from the CLI easier.
* Lifecycle: To phase out old application version, use a lifecycle policy.
* Extensions: All the parameters set in the UI can be configured with code using files.
* Cloudformation: under the hood, Elastic beanstalk relies on cloudformation
* Elastic Beanstalk allows us to clone an entire environment
* Elastic Beanstalk Migration: 
  * you can't change the ELB therefore you should create a new environment (no clone).
  * Decouple RDS
* Docker: 
  * Single:
    * provide dockerfile
  * Multi:
    * provide dockerun.aws.json (contain a task definition that will be on ECS)

## AWS CI/CD

* AWS CodeCommit
  * code
* AWS CodeBuild
  * Build & Test
  * Build instructions: buildspec.yml
* AWS CodeDeploy
  * Deploy
  * Provision
  * Servers must be running the CodeDeploy agent
* AWS CodePipeline
  * Orchestration
  * Consists of stages
  * Each pipeline stage can create artifacts
  * Each stage can have many action groups and each action groups can execute multiple actions in parallel

## AWS Cloudformation

* Declarative IaC to create AWS infrastructure
* Resources
* Parameters
  * !Ref
* Mappings
  * !FindInMap
* Outputs
  * It's the best way to perform some collaboration cross stack
  * Export !ImportValue
* Conditions
  * Control the creation of resources or outputs based on a condition
* Intrisic functions:
  * Fn::Ref :arrow_right: !Ref
  * Fn::GetAtt :arrow_right: !GetAtt
  * Fn::FindInMap :arrow_right: !FindInMap
  * Fn::ImportValue :arrow_right: !ImportValue
  * Fn::Join :arrow_right: !Join
  * Fn::Sub :arrow_right: !Sub
  * Fn::And :arrow_right: !And
  * Fn::Equals :arrow_right: !Equals
  * Fn::If :arrow_right: !If
  * Fn::Not :arrow_right: !Not
  * Fn::Or :arrow_right: !Or
* ChangeSets: what changes before it happens 
* NestedStacks: They allow you to isolate repeated patterns / common components in separated stacks and call them from other stacks
* StackSets: Create, update or delete stacks across multiple accounts or regions
* Drifts: Detects the manual changes over the stack created 

## AWS CloudWatch
### AWS CloudWatch Metrics

* Cloudwtach provides metrics for every service in AWS
  * Metrics by default
    * EC2 detail monitoring (metrics every 1 minute)
  * Custom metrics
    * Possibility to define and send your own custom metrics to CloudWatch
* High-Resolution Custom Metrics can have a minimum resolution of 1 second
* A CloudWatch Alarm set on a High-Resolution Custom Metric can be triggered as often as 10 seconds

### AWS CloudWatch Logs

* Log group
* Log stream
* By default, they never expire.
* Cloudwatch Logs can send logs to:
  * Amazon S3
  * Kinesis Data Streams
  * Kinesis Data Firehouse
  * AWS Lambda
  * Elasticsearch
* Metric filters can be used to trigger CloudWatch alarms.
* Subscription filter
* AWS CloudWatch Agent
  * Cloudwatch logs agent 
    * old version, only to send logs 
  * Unified agent (New version)
    * more granularity

### AWS CloudWatch Alarms

* Used to trigger notifications for any metric
* Alarm States:
  * OK
  * INSUFFICIENT_DATA
  * ALARM
* Targets:
  * SNS
  * EC2
  * Auto scaling
* Composite Alarms
  * monitoring the states of multiple others alarms
* you can test alarms using the CLI

### AWS CloudWatch Events

* Intercept events from AWS services
* EventBridge is the next generation of the cloudwatch

## AWS X-Ray

* Automated trace analisys & central service map visualization
* Latency, errors and fault analisys
* Request tracking across distrubuted systems
* How to enable it?
  * Your code must import the AWS X-Ray SDK
  * Install the X-Ray deamon or Enable X-Ray AWS integration
* Sampling Rules
* W-Ray writes APIs (Used by the X-Ray deamon)

## AWS ClousTrail

* Audit API calls made by users / services / AWS console
* Provides governance, compliance and audit for you AWS account 
* Enabled by default
* Events:
  * Management Events:
    * Read Events & Write Events on resources in your AWS account
  * Data Events
    * It's not enabled by default
    * Amazon S3 object-level activity (ex:GetObject,DeleteObject,PutObject)
  * Cloudtrail Insights
    * Detect unusual activity in your account
    * Analyzes normal management events to create a baseline :arrow_right: then continously analyzes write events to detect unusual patterns
* Retention
  * Stored for 90 days, to keep beyond this period log them to S3 and use Athena

## AWS Messaging

* SQS: Queue model
* SNS: pub/sub model
* Kinesis: real-time streaming model

## SQS

* Producer: send messages
* Consumer: poll messages
* Fully managed service, used to decouple applications
* You can configure the Amazon SQS message retention period to a value from 1 minute to 14 days. The default is 4 days.
* Standard
  * Can have duplicate messages
  * Can have out of order
* SQS queue access policies
  * Cross account access
  * Cross service access
* Message visibility timeout
  * After a message is polled by a consumer, it becomes invisible to other consumers
  * A conusmer could call the ChangeMessageVisibility API to get more time
* DLQ (Dead Letter Queue)
  * If a consumer fails to process a message the message goes back to the queue.
  * After the MaximumReceives threshold is exceeded, the message goes into DLQ
  * The message retention period should have enough time to process  or debug the messages
* Delay Queues
  * Delay a message (consumers don't see it inmmediately) up to 15 min.
* Long Polling: 
  * "Wait" (1 sec to 20 sec) for messages to arrive if there are none in queue
  * Long polling is preferable  to short polling 
  * ReceiveMessageWaitTimeSeconds
* SQS extended client:
  * Use S3 to save largest files and send only the metadata in SQS
* FIFO Queues
  * Messages will be delevired in order
  * 300 msg/s without batching, 3000 msg/s with
  * Exactly once
  * Deduplication
    * Content-based deduplication: will do a SHA-256 hash of the message body
    * Explicity provide a Message Deduplication ID
  * Message Grouping
    * Ordering  at the level of subsets of messages, specify different values for MessageGroupID
  * You can have as many consumers as "MessageGroupID" for your SQS FIFO queues.

## SNS

* Pub / Sub
* Send message to many receivers
* up to 12'500.000 subscription per topic
* Fan Out:
  * Push once in SNS, receive in all SQS queues or services subscribers
* Messaging filter: you can filter the messages that will be send to the subscibers
* FIFO: you can use FIFO in SNS
* Which of the following is NOT a supported subscriber for AWS SNS? --> Kinesis Data Firehose is now supported, but not Kinesis Data Streams.

## Kinesis

* Collect, process anda analyze steaming data in real-time

### Kinesis Data Streams

* Capture, process, and store data stream 
* Each stream have multiple shards
* Retetion between 1 to 365 days
* Once data is inserted in Kinesis, it can't be deleted (inmutability)
* Producers send records with the following information:
  * Partition key 
  * Data blob
* Consumers receive records with the following information:
  * Partition key 
  * Sequence no.
  * Data blob
* capacity mode
  * Provisioned mode
  * On-demand mode
* Hot partition: Use highly distributed partition key to avoid its
* ProvisionedThroughputExceeded:
  * Use highly distributed partition key
  * Retries with exponential backoff
  * Increase shards (Scaling)
* KCL - Kinesis Client Library:
  * Helps read from a Kinesis Data Stream with distributed applications sharing the read workload
  * Progress is checkpointed into DynamoDB
* Shard Spliting: use to divide a hot shard
* Merging Shards: Group two shards with low traffic
* Enhanced Fanout consumer --> To increase the read throughput for Kinesis Data Streams consumers up to 2 MB/s per consumer per shard

### Kinesis Data Firehouse

* Load data streams into AWS data stores
* Serverless
* Pay for data going through Firehouse
* Near real time
* Supports custom data transformations using AWS Lambda
* Can send failed or all data to a backup S3 bucket
### Kinesis Data Analytics

* Analyze data streams with SQL or Apache Flink

### Kinesis Video Streams

* Capture, process, and store video stream 
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
* Permissions
  * Lambda Execution Role (IAM Role)
    * Grants the lambda function permissions to AWS services / resources
    * There's sample managed policies for lambda (e.g AWSLambdaBasicExecutionRole)
    * Best practice: create one Lambda Execution Role per function
  * Resource based policy
    * To give other  accounts and AWS services permission to use your lambda resources.
* Synchronous invocations
  * Results is returned right away.
  * Error handling must happen client side
  * Some services:
    * ALB, Api gateway, Cloudfront (Lambda@Edge), S3 Batch, cognito, step functions, Lex, Alexa, Kinesis Data Firehouse.
* Asynchronous invocations
  * you don't need to wait for the result
  * Some services: S3, SNS, Cloudwatch events / event bridge, codecommit, codepipeline
  * you can configurate a DLQ in the Lambda (also you can configure retries to the messages before send them to the DLQ).
* Event Source Mapping
  * Records need to be polled from the source.
  * Lambda is invoked synchronously.
  * Services:
    * Streams:
      * Kinesis Data Streams.
      * DynamoDB streams.
    * Queue
      * SQS & SQS FIFO queue
* Lambda Destinations
  * Send the result of an asynchronus invocation or the failure of an event mapper into somewhere
  * Asynchronus destinations:
    * Amazon SQS
    * Amazon SNS
    * AWS Lambda
    * Amazon EventBridge Bus
  * Event Source Mapping Destinations:
    * Amazon SQS
    * Amazon SNS
* We can use environment variables
* Lambda Logging & Monitoring:
  * CloudWatch logs
  * CloudWatch Metrics
* Tracing with X-Ray
  * Runs the  X-Ray daemon for you.
  * Uses env to communicate with X-Ray
    * AWS_XRAY_DAEMON_ADDRESS
* Performance
  * from 128 MB to 10 GB in 1MB increments
  * At 1792 MB a function has the equivalent of one full vCPU
  * If your application is CPU-bound (computation heavy), increase RAM
  * Timeout: default 3 seconds, maximum 900 seconds (15 minutes)
  * Lambda execution context:
    * temporary runtime environment that initializes any external dependencies of your lambda code
    * it is maintained for some time in anticipation of anothe lambda function invocation
      * A good practice is initialize outside the handler (for example DB connection)
    * It includes the /tmp directory (Max size 10Gb)
      * The directory content remains when the execution context is frozen.
      * for permanent persistent of object use S3
* Concurrency limit: up to 1000 (it depends of AWS account) concurrent executions (all lambdas functions)
* Provisioned Concurrency: The cold start never happens and all invocations have low latency
* Dependencies: You need to install the packages alongside your code and zip it together (if less than 50MB else S3 first)
* Lambda & Cloudformation:
  - Inline
  - S3
* Lambda Layers:
  * Custom runtimes
  * Externalize Dependencies to re-use them
* Lambda Container Images: Base image must implement the Lambda Runtime API
* AWS Lambda Versions and Aliases:
  * Version = code + configuration (nothing can be changed (immutable))
  * Alias = pointers to lambda function versions
    * CodeDeploy can help you automate traffic shift for lambda aliases
* Deployment Limits:
  * Lambda function deployment size (compressed .zip): 50MB
  * size uncompressed deployment: 250MB
  * size of environments variables: 4KB

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

### Lambda Networking

* By default, your lambda function is launched outside your own VPC (in an AWS-owned VPC)\
* Lambda in VPC:
  * Define VPC
  * Lambda will create a ENI
  * AWSLambdaVPCAccessExecutionRole
  * By default Lambda does not have internet access
    * You need to use a NAT gateway
    * You can use VPC endpints to privately access AWS services without NAT

## AWS Serverless - DynamoDB

* It's a NoSQL database fully managed, highly available with replication across multiple AZ.
* NoSQL databases scale horizontally.
* Each table has a primary key (must be decided at creation time).
* Maximum size of an item is 400KB.
* Primary Keys:
  * Partition Key (HASH) 
  * Partition Key + Sort Key (HASH + RANGE)
* you can choose between strongly consistent read vs eventually consistente read 
* Capacity Modes:
  * Provisioned mode:
    * you need to plan capacity beforehand 
    * WCU: Write capacity units :arrow_right: one WCU represent one write per second for an item up to 1 KB
    * RCU: Read capacity units:
      * :arrow_right: one RCU represent one strongly consistent read per second for an item up to 4 KB
      * :arrow_right: one RCU represent two eventually consistent reads per second for an item up to 4 KB
  * On-demand mode
* Partitions internal
  * WCUs and RCUs are spread evenly across partitions
* Basic API's:
  * PutItem
  * UpdateItem
  * GetItem
  * Query
  * Scan
  * DeleteItem
  * DeleteTable
  * BatchWriteItem
  * BatchGetItem
* Local Secondary Index (LSI)
  * Alternative sort key
  * Uses the WCUs and RCUs of the main table
* Global Secondary Index (GSI)
  * Alternative primary key
  * Global Secondary Index (GSI) uses an independent amount of RCU and WCU and if they are throttled due to insufficient capacity, then the main table will also be throttled.
* PartiQL: use SQL-like syntax to manipulate DynamoDB
* Optimistic Locking:
  * Which feature of DynamoDB allows you to achieve Optimistic Locking :arrow_right: Conditional Writes.
* DynamoDB Accelerator (DAX)
  * Fully-managed, highly available, seamless in-memory cache for DynamoDB
  * Solves the "hot key" problem
  * 5 minutes TTL for cache 
  * ElasticCache vs DAX: 
    * ElastiCache: Store aggregation result
    * DAX: Individuals object cache / Query & Scan cache
* Dynamo Streams: allows you to capture a time-ordered sequence of item-level modifications in a DynamoDB table. It's integrated with AWS Lambda so that you create triggers that automatically respond to events in real-time.
* Dynamo TTL: 
  * Automatic delete items after an expiry timestamp
* Dynamo Transactions:
  * Provides Atomicity, Consistency, Isolation and Durability (ACID)
  * Consumes 2x WCUs & RCUs
* DynamoDB as session State cache
* Partition Strategies:
  * A strategy that allows better distribution of items evenly across partitions is add  a suffix to a partition key value
* Writes types
  * Cocurrent writes.
  * Conditional writes.
  * Atomic writes.
  * Batch writes.

## AWS Serverless - API Gateway

* No infrastructure to manage
* Handle API Versioning
* Integrations:
  * Lambda Function
  * HTTP (ALB)
  * AWS Service
* Endpoints types:
  * Edge-Optimized (Default)
  * Regional
  * private
* Security:
  * User Authentication through:
    * IAM Roles
    * Amazon cognito
    * Custom Authorizer
  * Custom Domain Name HTTPS (through ACM)
* We can create a stage variable to indicate the corresponding Lambda alias.
* Integration types:
  * Mocks 
  * HTTP / AWS 
    * Setud data mapping using mapping templates for request and response
  * AWS_PROXY (Lambda Proxy)
  * HTTP_PROXY (HTTP Proxy
* API gateway has integration with Open API spec to import or export an API through  Open API spec
* Logging & Tracing:
  * Cloudwatch Logs
  * X-ray
* Cloudwatch Metrics
  * CacheHitCount  & CacheMissCount
  * Count
  * IntegrationLatency
  * Latency
  * 4XX & 5XX errors
* API Gateway Throttling
  * 10000 rps across all API
* You can configure CORS on API gateway
* Security:
  * IAM Permissions
  * Resource Policies
  * Cognito User Pools
  * Lambda Authorizer
* HTTP APIs VS REST APIs: HTTP is cheaper and does not support resource policies, only support proxy (Not support data mapping)
* WebSocket
  * Is for two-way communication
  * Routing: It is used to make sure that you can route to a specific backend based on the routing expression

## AWS Serverless - SAM: Serverless Application Model

* Framework for developing and deploying serverless applications (it is built on CloudFormation)
* All the configuration is YAML code (you can recognize this one finding the "Transform" property in the YAML file)
* Package and deploy:
  * aws cloudformation package / sam package
  * aws cloudformation deploy / sam deploy
* Deployment process:
  * sam build: crate a cloud formation yaml
  * sam package: upload the cloudformation template and code to S3
  * sam deploy: deploy application from S3
* SAM CLI: Run lambda locally 
* SAM Policy Templates:
    * List of templates to apply permissions to your lambda functions example:
      - S3ReadPolicy
      - SQSPollerPolicy
      - DynamoDBCrudPolicy
* SAM is integrated with codedeploy to do deploy to Lambda aliases
* Serverless Application Repository (SAR):
  * Managed repository for serverless applications
  * sam publish command

## AWS Cloud Development Kit (CDK)

* Define your cloud infrastructure using a familiar language: JS (TS), Python, Java and .Net
* The code is "compiled" into a CloudFormation template (JSON/YAML)
* You can deploy infrastructure and application runtime code together:
  * Great for lambda functions
  * Great for Docker containers in ECS / EKS
* Commands:
  - cdk bootstrap (bootstrap the CDK application)
  - cdk synth (optional to synthesize as a cloudformation template)
  - cdk deploy (deploy the CDK stack)
  - cdk destroy (destroy the stack)

## AWS Cognito

* To give our users an identity so that they can interact with our application
* Cognito User Pools:
  * Create a serverless database of user for your web & mobile apps
  * simple login, password reset, email & phone verification, MFA, Federated identities (Facebook, google, SAML), JWT
  * integrations: AWS API gateway & ELB
* Cognito Identity Pools:
  * Provide users access to AWS
* Cognito Sync

## AWS step functions

* AWS Step Functions is a low-code visual workflow service used to orchestrate AWS services, automate business processes, and build Serverless applications. 
* Model your workflows as states machines (one per workflow)
* Written in Json
* SDK call, API gateway, Event bridge
* Step function - states:
  * Task state: do some work in your state machine
  * parallel state: Begin parallel branches of execution
* Error Handling
  * you can use RETRY or CATCH
* Types:
  - Standard
  - Express

## AWS AppSync

* AWS AppSync is a fully managed service that makes it easy to develop GraphQL APIs by handling the heavy lifting of securely connecting to data sources like DynamoDB, Lambda, and more.
* Is a managed service that uses GraphQl
* Retrieve data in real-time with WebSocket
* For mobile apps: Local data access & data synchronization

## AWS Amplify

* Set of tools to create mobile and web applications
* Important features:
  * Authentication
  * DataStore
* Amplify Studio
* Amplify CLI
* Amplify Libraries
* Amplify Hosting
