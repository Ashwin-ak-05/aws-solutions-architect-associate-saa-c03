# AWS Solutions Architect Associate (SAA-C03)
## Exam Essentials Guide

**By Chetan Agrawal** ([www.awswithchetan.com](http://www.awswithchetan.com))
Udemy Instructor | Ex. Senior Solutions Architect at AWS

*All rights reserved © www.awswithchetan.com*

---

## Preface

- This guide is a concise revision companion designed to reinforce the most important exam-relevant concepts across AWS services. It summarizes key ideas, behaviors, limits, and best practices exactly as covered in the course lectures, helping you quickly recall what matters most during the exam.
- The content is not a replacement for lectures or hands-on practice, but a high-impact reference for final revision, concept clarity, and exam readiness. Each slide focuses on frequently tested patterns, common traps, and decision-making cues that appear in AWS certification exams.
- Use this guide for last-mile preparation, quick refreshers, and confidence building before the exam.

---

## Table of Contents

1. [IAM](#iam--exam-essentials)
2. [EC2](#ec2---exam-essentials)
3. [EBS](#ebs---exam-essentials)
4. [EC2 Advanced](#ec2-advanced--exam-essentials)
5. [Elastic Load Balancer](#elastic-load-balancer---exam-essentials)
6. [Autoscaling Group](#autoscaling-group---exam-essentials)
7. [VPC and Networking](#vpc-and-networking---exam-essentials)
8. [AWS Direct Connect](#aws-direct-connect)
9. [Amazon S3](#amazon-s3--exam-essentials)
10. [AWS Container Services](#aws-container-services--exam-essentials)
11. [AWS Databases - Summary](#aws-databases---summary)
12. [Amazon RDS](#amazon-rds--exam-essentials)
13. [AWS Databases - Exam Essentials](#aws-databases--exam-essentials-1)
14. [AWS Databases - Exam Scenarios](#aws-databases---exam-scenarios)
15. [Big Data Analytics Services](#big-data-analytics-services--exam-essentials)
16. [AWS Analytics Services - Match the Pairs](#aws-analytics-services--match-the-pairs)
17. [AWS Machine Learning and AI](#aws-machine-learning-and-ai--exam-essentials)
18. [AWS Edge Networking](#aws-edge-networking--exam-essentials)
19. [Route 53 (DNS)](#route-53-dns--exam-essentials)
20. [Serverless: Amazon API Gateway](#serverless-amazon-api-gateway--exam-essentials)
21. [Serverless: AWS Lambda](#serverless-aws-lambda--exam-essentials)
22. [SQS, SNS, EventBridge](#sqs-sns-eventbridge--exam-essentials)
23. [ACM](#acm--exam-essentials)
24. [KMS](#kms--exam-essentials)
25. [Secrets Manager](#secrets-manager--exam-essentials)
26. [AWS Infrastructure Deployment & DevOps](#aws-infrastructure-deployment--devops)
27. [AWS Elastic Beanstalk](#aws-elastic-beanstalk--exam-essentials)
28. [Logging and Monitoring](#logging-and-monitoring--exam-essentials)
29. [AWS Systems Manager](#aws-systems-manager--exam-essentials)
30. [AWS CloudTrail](#aws-cloudtrail--exam-essentials)
31. [AWS Config](#aws-config--exam-essentials)
32. [AWS Security Services](#aws-security-services--exam-essentials)
33. [Storage and Data Migration](#storage-and-data-migration--exam-essentials)
34. [AWS Storage Services - Match the Pairs](#aws-storage-services--match-the-pairs)
35. [Migration and Disaster Recovery](#migration-and-disaster-recovery---exam-essentials)
36. [AWS Multi-account Management](#aws-multi-account-management--exam-essentials)
37. [AWS Billing and Cost Management](#aws-billing-and-cost-management--exam-essentials)

---

## IAM – Exam Essentials

### IAM Fundamentals
- IAM is a global service where permissions apply across all AWS regions and services.
- Permissions are managed using IAM Policies (JSON) where you define Effect, Action, Resource, and optional Conditions.
- By default, any access is denied; IAM policies should explicitly Allow the permissions for permitted actions.
- An explicit Deny in the policy overrides Allows.
- Root user has unrestricted access and should only be used for account level settings and other related activities.

### IAM Users, Groups, and Roles
- Users represents long-term identities (typically humans).
- Groups are collections of users which are used to simplify permissions management.
- Roles provide temporary access for AWS services, Users, or External identities.
- Roles are assumed via AWS STS, providing temporary credentials (AccessKeyID, SecretAccessKey, and SessionToken).
- AWS STS APIs: `AssumeRole` for AWS Services or cross-account access, `AssumeRoleWithSAML` for SAML federation (AD), `AssumeRoleWithWebIdentity` for OIDC/Web identity federation (google, facebook)

### IAM Policy Types
- Identity-based policies → Attached to users, groups, or roles.
- Resource-based policies → Attached directly to resources (S3, KMS, SQS, Lambda).
- AWS Managed policies → Reusable; can be AWS-managed or customer-managed.
- Inline policies → Embedded directly in one identity (unique to it).

### IAM Policy Conditions
- Add fine-grained control using condition keys:
  - `aws:RequestedRegion` → Restrict region
  - `aws:SourceIp` → Restrict IP range
  - `aws:MultiFactorAuthPresent` → Enforce MFA
- Combine with operators like `StringEquals`, `Bool`, `IpAddress`.

### Additional Features and Tools
- IAM Permission boundaries → Define the maximum allowed permissions for an identity.
- IAM Policy Simulator is used to evaluate and troubleshoot whether IAM policies allow or deny specific actions.
- IAM Policy Generator helps create least-privilege IAM policies based on observed permissions usage.
- IAM Credentials Report provides an account-level report of all IAM users and the status of their credentials.

### IAM Best Practices
- Apply Least Privilege — grant only required permissions.
- Enable MFA on all users (especially root user).
- Use IAM roles for applications instead of embedding IAM User AccessKey/SecretAccessKey.
- Rotate credentials regularly.
- Monitor Unused credentials, permissions with IAM Access Analyzer.

---

## EC2 - Exam Essentials

- **EC2** - Virtual Machine in the AWS cloud hosted in an AZ within the Region
- **EC2 Configuration options** - Instance Type/Size (CPU + RAM) + AMI (OS) + Storage (EBS/Instance store) + VPC + Security Groups + SSH key-pair (+ IAM Role, User data, Tags)
- **EC2 Block Storage options** – Persistent storage - Elastic Block Storage (EBS), Non persistent or ephemeral storage - Instance Store
- **EC2 Networking and IP address** – Amazon VPC, Private IP vs Public IP vs Elastic IP
- **EC2 Security groups** – Contains inbound rules and outbound rules (Protocol, Port, IP range), Stateful
- **EC2 initialization** – EC2 User Data scripts
- **Login Options:** SSH, RDP *(there are more options to connect to EC2 & we will cover them later)*
- **Pricing Options** - On-Demand, Spot, Reserved, Savings Plan
- **Tenancy options** – Shared, Dedicated – Dedicated Instances & Dedicated Host
- **Tags** – Name-value key pairs, useful for filtering, deployment, User IAM permissions, cost allocation etc.

### Additional EC2 Essentials
- EC2 AMIs are Region-specific and must be copied across Regions before use in another Region.
- User data is used to run bootstrap scripts automatically when an EC2 instance launches.
- Security groups act as stateful, instance-level firewalls that control inbound and outbound traffic.
- Security groups allow traffic by default only if explicitly permitted and have no explicit deny rules.
- Instance store provides high-performance, temporary storage that is lost when the instance stops or terminates.
- Spot Instances provide the lowest-cost compute for fault-tolerant and interruption-tolerant workloads.
- On-Demand Instances are best for short-term or unpredictable workloads with no long-term commitment.
- Savings Plans reduce compute costs for steady workloads across EC2 and other compute services such as Fargate.
- EC2 instances use shared tenancy by default, with dedicated tenancy available for compliance or isolation needs.
- EC2 supports tagging, and IAM policies can restrict actions based on resource tags.
- EC2 Instance Connect provides secure, temporary SSH access without managing long-term SSH keys.

---

## EBS - Exam Essentials

### EBS Basics
- EBS (Elastic Block Store) provides persistent block storage for EC2 instances.
- Each volume is automatically replicated within its Availability Zone (AZ) to protect against hardware failure.
- Volumes are independent of instance lifecycle - data persists even after the instance stops or terminates.
- EBS volumes must be in the same AZ as the EC2 instance.
- EBS volume size, IOPS can be changed dynamically (does not require to stop the instance).

### Volume Types and Features
- **gp3** (General Purpose SSD) → Default choice; Balance of cost and performance; customizable IOPS & throughput.
- **io1/io2 Block Express** (Provisioned IOPS SSD) → For critical, latency-sensitive databases; supports Multi-Attach.
- **st1** (Throughput Optimized HDD) → Big data, log processing, streaming workloads (sequential I/O).
- **sc1** (Cold HDD) → Low-cost, infrequent access workloads (archival).
- gp3 provides baseline performance of 3000 IOPS and supports additional as per size of the volume.
- AWS recommends to migrate from gp2 to gp3 volume. Migration can be done without interruption.

### EBS Snapshots
- Snapshots are point-in-time backups stored in Amazon S3.
- Snapshots are incremental - only changed blocks are saved after the first full snapshot.
- Can be used to create new volumes or copy across regions for DR setups.
- Can share snapshots with other AWS accounts or make them public.
- Amazon Data Lifecycle Manager (DLM) automates EBS snapshot creation and retention.

### EBS Advanced Features
- **Multi-Attach** (io1/io2 only): Same volume attached to multiple EC2s in one AZ (for HA clusters).
- **EBS-Optimized Instances:** Dedicated bandwidth for consistent performance.

### EBS Encryption
- EBS encryption uses KMS keys (CMKs). By default, uses AWS-managed CMK (aws/ebs).
- You can use a Customer Managed CMK for compliance control.
- If CMK is disabled → temporary lock; if deleted → permanent data loss.
- Encryption applies at rest, in transit, and to snapshots automatically.

---

## EC2 Advanced – Exam Essentials

- EC2 Image Builder automates the creation, testing, and distribution of standardized AMIs.
- EC2 Image Builder helps reduce manual AMI maintenance and improves security and consistency.
- EC2 hibernation preserves in-memory (RAM) state when an instance is stopped and resumes faster on start.
- Hibernation is supported only for specific instance families and sizes and requires an EBS-backed root volume.
- Elastic Network Interfaces (ENIs) are virtual network cards that can be attached, detached, or moved between instances.
- Multi-homed EC2 instances use multiple ENIs for traffic separation, high availability, or network appliances.
- Placement groups control how EC2 instances are placed on underlying hardware for performance or availability.
- Cluster placement groups provide low-latency, high-throughput networking for tightly coupled workloads.
- Spread placement groups place instances on distinct hardware to reduce correlated failures.
- Partition placement groups isolate groups of instances, commonly used for large distributed systems.
- Instance Metadata Service v2 (IMDSv2) provides instance metadata using session-based authentication.
- IMDSv2 is used by applications on EC2 to securely retrieve temporary IAM role credentials from the instance metadata.

---

## Elastic Load Balancer - Exam Essentials

- **ALB:** Layer 7, supports content-based routing (host/path/header/query), WAF, Lambda, WebSockets, gRPC, and adds X-Forwarded-For for client IP.
- **NLB:** Layer 4, preserves client IP, supports TCP/UDP/TLS, static/EIPs, and Proxy Protocol v2 for forwarding client metadata.
- **GWLB:** Layer 3, uses GENEVE 6081, scales network appliances (firewalls/IDS); transparent IP forwarding.
- **Cross-Zone Load Balancing:** ALB = ON by default and no inter-AZ data transfer cost, NLB = OFF by default (Inter-AZ data transfer cost if enabled).
- **SNI:** Single HTTPS listener, multiple SSL/TLS certificates (multi-domain).
- **Connection Draining / Deregistration Delay:** Allows in-flight requests to complete (default 300s).
- **Dual-Stack:** ALB & NLB support IPv4 + IPv6; GWLB IPv4-only.
- **Stickiness:** ALB via cookie (Duration based and Application based); NLB via Source IP.
- **Health Checks:** Defined per Target Group

> **Exam Tip:** ALB = Smart routing; NLB = Performance & Static IP; GWLB = Network Appliance scaling

---

## Autoscaling Group - Exam Essentials

### Scaling Types
- **Target Tracking** → auto-maintains metric (e.g., CPU = 50%) (no manual alarms).
- **Step/Simple Scaling** → needs CloudWatch alarms; Step = graduated actions.
- **Scheduled Scaling** → time-based.
- **Predictive Scaling** → forecasted demand.

### Other Key Points
- **Launch Template** → required; supports Mixed Instances Policy (On-Demand + Spot).
- **Lifecycle Hooks** → run scripts on launch/terminate; **Warm Pools** → faster scale-out.
- **Instance Refresh** → rolling update of instances for new AMI/config.
- **Health Checks:** EC2 or ELB; ELB health check is better as it checks application health. Autoscaling group will replace the unhealthy instances automatically.
- **Termination Policy:** decides which instance to remove (first oldest launch config → oldest instance → AZ balance).
- **Cooldown:** Prevents rapid re-scaling (default 300s).

> **Note:** Autoscaling group can be used without ELB and scaling can be triggered based on any CloudWatch Metrics like SQS queue depth etc.

---

## VPC and Networking - Exam Essentials

### VPC Fundamentals
- VPC is a logically isolated private network in AWS.
- VPC has a private address range called CIDR (IPv4 and IPv6).
- VPC supports Public/Elastic IPs for public connectivity and Private IPs for private connectivity.
- Internet Gateway connects VPC to the internet.
- NAT gateway provides outbound internet access to instances in the Private subnet (IPv4 traffic).
- VPC offers 2 types of firewalls – Security group and Network ACL. Security Groups are stateful; NACLs are stateless and subnet-based.
- AWS creates default VPC in each region with Public subnet in each AZ. Default VPC can be deleted and re-created.
- Troubleshooting connectivity → Usually SG/NACL/RT issues.

### VPC Endpoints
- VPC Gateway Endpoints (S3/DynamoDB only): Free to use. Need to modify subnet route table.
- VPC Interface Endpoints (powered by AWS PrivateLink): Provisions ENIs, Uses Security Group, Enable private DNS.
- VPC interface endpoint can be accessed from remote networks like over VPC peering, VPN or DX.
- Gateway endpoint can only be accessed from within the VPC. Can't be accessed from remote networks.

### VPC Private Connectivity
- **VPC Peering:** One-to-one, no transitive routing, same/other region.
- **Transit Gateway:** hub-and-spoke; scalable, centralized routing, supports thousands of VPCs.
- TGW supports inter-region peering, shared across accounts, and ECMP with multiple VPN tunnels for aggregated bandwidth.
- For multi-VPC scalable architecture → Prefer TGW over Peering.

### Hybrid Connectivity
- **Site-to-Site VPN:** IPsec over internet; quick setup; supports BGP; encrypted.
- **Client VPN:** secure client-based access to VPC; scalable and managed by AWS.
- **Direct Connect (DX):** private, consistent, high-bandwidth; not encrypted by default.
- DX Gateway connects DX to multiple VPCs across multiple regions.
- For DX traffic encryption → use VPN over DX or MacSec (Layer 2) on supported DX connections.
- DX Failover architecture: DX (primary) + VPN (backup) using BGP for automatic failover.
- Four DX connections across different DX locations and Devices for maximum resiliency.

### VPC Traffic Monitoring
- **VPC Flow Logs:** metadata only; ideal for troubleshooting, blocked traffic analysis, compliance audits, anomaly detection, CloudWatch/S3 storage.
- **VPC Traffic Mirroring:** full packet capture; deep inspection, IDS/IPS, threat forensics, debugging app-level issues, forwarding to security tools.
- Flow Logs => lightweight metadata; Traffic Mirroring => Heavy packet-level analysis.

### IPv6 in VPC
- IPv6 = globally unique, publicly routable; VPC gets /56, subnets get /64.
- NAT Gateway does not support IPv6 → use Egress-Only IGW for outbound-only IPv6.
- SGs and NACLs require separate IPv6 rules.
- IPv6 often used for IoT, large-scale networks, or avoiding NAT bottlenecks.

---

## AWS Direct Connect

- AWS Direct Connect provides private, dedicated network connectivity between on-premises and AWS.
- Uses BGP for Dynamic routing.
- Offers high bandwidth options: 1 Gbps, 10 Gbps, 100 Gbps on dedicated ports, and hosted DX from 50 Mbps to multi-Gbps.
- Traffic is not encrypted by default; for encryption use VPN over DX or MACsec (where supported).
- It takes few weeks to month to establish Direct Connect connectivity (hence need advance planning).
- Use Private VIF to access a VPC, Public VIF to privately access AWS public services like S3 or DynamoDB and Transit VIF to connect to Transit Gateway (via DX Gateway).
- Use DX Gateway to connect a single DX to multiple VPCs across multiple Regions.
- Provides reduce data transfer costs (e.g. $0.02/GB as compared to internet charge of $0.09/GB).
- **Architecture:** For high availability, deploy multiple DX connections in different DX locations.
- **Architecture:** You can use DX+VPN for resilient connectivity where VPN acts as a failover network.

---

## Amazon S3 – Exam Essentials

### S3 Storage Classes
- S3 Standard is default storage. It's multi-AZ and designed to provide 11 9's of durability. Best for frequently accessed data.
- Standard-IA / One Zone-IA for infrequently accessed data. 30-day minimum duration; retrieval fee applied; One-Zone is single AZ which is ideal for reproducible/non-critical data.
- Intelligent-Tiering automatically moves objects across tiers for small monthly monitoring fee; no retrieval charges. Use in case of un-predictable access pattern.
- Glacier Instant Retrieval / Flexible Retrieval / Deep Archive – Cheaper archival storage; Data retrieval times vary (milliseconds, minutes–hours, 12–48 hours). Minimum 90/180 days duration charges depending on tier.
- S3 Express One Zone – Ultra-low latency; AZ-specific; high throughput workloads; Can co-locate application instances by selecting the AZ.

### S3 Storage Class Comparison

| | S3 Standard | S3 Intelligent-Tiering | S3 Standard-IA | S3 One Zone-IA | S3 Glacier Instant Retrieval | S3 Glacier Flexible Retrieval | S3 Glacier Deep Archive |
|---|---|---|---|---|---|---|---|
| **Use cases** | General purpose storage for frequently accessed data | Automatic cost savings for data with unknown or changing access patterns | Infrequently accessed data that needs millisecond access | Re-creatable infrequently accessed data | Long-lived data accessed a few times per year with instant retrievals | Backup and archive data that is rarely accessed and low cost, with expedited/standard/bulk retrieval options | Archive data that is very rarely accessed and very low cost |
| **First byte latency** | milliseconds | milliseconds | milliseconds | milliseconds | milliseconds | minutes or hours | hours |
| **Durability** | 99.999999999% (11 nines) in a region | Durability depends on storage class where object is moved | 99.999999999% (11 nines) in a region | 99.999999999% (11 nines) within a Single AZ | 99.999999999% (11 nines) in a region | 99.999999999% (11 nines) in a region | 99.999999999% (11 nines) in a region |
| **Availability** | 99.99% | 99.90% | 99.90% | 99.50% | 99.90% | 99.99% | 99.99% |
| **Availability SLA** | 99.90% | 99% | 99% | 99% | 99% | 99.90% | 99.90% |
| **Availability Zones** | ≥3 | ≥3 | ≥3 | 1 | ≥3 | ≥3 | ≥3 |
| **Min storage duration charge** | N/A | N/A | 30 days | 30 days | 90 days | 90 days | 180 days |
| **Retrieval charge** | N/A | N/A | per GB retrieved | per GB retrieved | per GB retrieved | per GB retrieved | per GB retrieved |

*Reference: https://aws.amazon.com/s3/storage-classes/*

### S3 Security
- Block Public Access overrides bucket policies and ACLs; Prevents accidental public exposure.
- Bucket Policies controls access to S3 buckets; Use "Principal" for cross-account access.
- For S3 Server-side encryption, use SSE-KMS for audit trail. Requires `kms:GenerateDataKey` and `kms:Decrypt` IAM permissions for S3 to perform encryption and decryption.
- Object Lock for WORM requirements; Prevents objects from being deleted or changed for the specified duration. Objects locked with compliance mode can not be changed. Legal Hold makes sure that objects are not deleted indefinitely (or until Legal Hold removed).
- MFA Delete prevents object and versions deletion. Only root user can enable/disable.
- Bucket Versioning protects against accidental overwrite/delete but costs more as multiple copies of the objects are stored. For using many other features like Object Lock, Replication etc. the versioning must be enabled on the bucket.

### S3 Management
- Lifecycle Rules transitions only hotter to colder storage (follows waterfall model); Lifecycle cannot transition IA → Standard; Minimum duration charges apply for IA/Glacier tiers; Async processing.
- Replication (CRR/SRR) does NOT replicate existing objects. Use Batch Replication for replicating existing objects. Do not replicate deletion, optionally delete marker replication can be enabled.
- Replication Time Control (RTC) ensures 99.99% objects replicated within 15 minutes (SLA).
- Replication requires versioning enabled on source and destination.
- S3 Batch Operations to perform operations on millions of objects; needs manifest like S3 inventory or CSV file; Can invoke Lambda for each object for processing.

### S3 Events and Integrations
- Event Notifications triggers Lambda/SNS/SQS; prefix/suffix filters; not all event types supported but it's faster, simple and it's free to use (charges for targets still apply).
- Amazon EventBridge for all S3 API events; Richer routing; slightly higher latency; charges apply.
- For advanced event routing and workflow execution, use Amazon EventBridge.

### Logging and Reporting
- S3 Server Access Logs records access requests to S3 which can further be analyzed using Athena.
- IAM Access Analyzer detects publicly or cross-account accessible buckets.
- S3 Storage Lens provides AWS Organization wide S3 usage & activity analytics.
- Use S3 Inventory for periodic object listing + metadata; Useful for S3 batch operations.

### Performance
- Prefix Partitioning – S3 auto-scales prefixes; Objects with the same prefix may still share throughput limits.
- Multipart Upload – Must for files >5GB; faster uploads; parallel uploads.
- Byte Range Fetch – Parallelize downloads; resume partial downloads.
- Transfer Acceleration – Speeds uploads using AWS edge network; best for long-distance large size uploads.
- CloudFront + S3 – Lower latency + caching; helps offload GETs from S3, lower Data transfer out charges.

### Additional Features
- Static Website Hosting – Only on public buckets; no HTTPS directly (use CloudFront).
- Requester Pays – Requester pays data transfer and request costs.
- Pre-Signed URLs – Temporary access; good for upload/download; max 7-day expiry (SigV4).
- CORS – Browser only security feature, required only for browser cross-origin fetch.
- Access Points – Dedicated hostnames + Access point policies for different apps/teams; supports VPC access points.

---

## AWS Container Services – Exam Essentials

### Amazon ECS
- Amazon ECS is a managed container orchestration service for running Docker containers on AWS.
- ECS supports two launch types: EC2 (you manage instances) and Fargate (serverless containers).
- An ECS task definition defines how containers run, including image, CPU, memory, ports, and roles.
- ECS uses the EC2 instance role for EC2 launch type and the task execution role for Fargate to pull images, write logs, and fetch secrets.
- The ECS task role is assumed by the container to access AWS services e.g. S3, DynamoDB etc.
- ECS Service Auto Scaling scales the number of running tasks (not EC2 instances). Supports different scaling policies e.g. Target tracking, Step, scheduled etc. Also integrates with Amazon CloudWatch.
- For EC2 launch type, ECS capacity providers are used to automatically scale EC2 capacity.
- ECS services integrate with ALB or NLB for load-balanced, long-running applications.
- ECS tasks can be run in event-driven fashion by using Amazon EventBridge.
- Use Amazon EFS for shared storage that can be mounted by multiple tasks across EC2/Fargate.

### Amazon EKS
- Amazon EKS is a managed service for running Kubernetes clusters on AWS.
- AWS manages the Kubernetes control plane, while customers manage worker nodes.
- It is commonly used when teams need standard Kubernetes APIs and tooling.

### AWS Fargate
- AWS Fargate is a serverless compute engine for containers used with Amazon ECS and Amazon EKS.
- With Fargate, you do not manage EC2 instances, AMIs, or clusters, as AWS provides the compute capacity automatically.
- You pay for vCPU and memory per task or pod, only while containers are running.
- Fargate requires `awsvpc` networking, so each task or pod gets its own ENI and security groups.
- Scaling with Fargate involves scaling tasks or pods only, not infrastructure.
- It is best suited for stateless, event-driven, or microservice workloads.
- Fargate is commonly used when minimal operational overhead is a priority.

### Amazon Elastic Container Registry (Amazon ECR)
- Amazon ECR is a fully managed container image registry.
- It integrates natively with ECS, EKS, and Fargate.
- ECR supports private and public repositories, IAM-based access control, and image lifecycle policies.
- ECR can scan container images for vulnerabilities using Amazon Inspector.

### Amazon ECS Anywhere
- Amazon ECS Anywhere allows ECS workloads to run on on-premises or external servers using the ECS control plane in AWS.

### Amazon EKS Anywhere
- Amazon EKS Anywhere allows Kubernetes clusters to run on on-premises infrastructure using AWS-validated tooling.

---

## AWS Databases - Summary

| Category | Sub-type | AWS Service | Compatible / Related With |
|---|---|---|---|
| Relational Database | Open source | Amazon RDS | PostgreSQL, MySQL, MariaDB |
| Relational Database | Commercial | Amazon RDS | Oracle, SQL Server, IBM DB2 |
| Relational Database | AWS native | Amazon Aurora | PostgreSQL, MySQL — plus Aurora Global Database |
| Database Cache | Advanced | Amazon ElastiCache | ElastiCache for Redis |
| Database Cache | Simple / In-memory | Amazon ElastiCache | ElastiCache for Memcached |
| NoSQL Database | Key-value | Amazon DynamoDB | DAX, Streams, Global Table |
| NoSQL Database | Document (JSON) | Amazon DocumentDB | => MongoDB |
| Graph Database | Nodes, Edges, Relationships | Amazon Neptune | — |
| Timeseries Database | — | Amazon Timestream | => InfluxDB |
| Ledger Database | — | Amazon QLDB | — |
| Wide Column | — | Amazon Keyspaces | => Cassandra |

---

## Amazon RDS – Exam Essentials

- Fully managed database service with automated provisioning and OS patching.
- AWS provides the DNS for accessing the database instance e.g. `mydb.xxxxx.us-east-1.rds.amazonaws.com`
- Automated backups with Point-in-Time Restore (PITR) + manual snapshots.
- Built-in monitoring via CloudWatch, Enhanced Monitoring, and Performance Insights.
- Read Replicas to scale read-heavy workloads and improve read performance.
- Multi-AZ deployments for high availability and disaster recovery.
- 30 mins maintenance windows for controlled patching and minor upgrades.
- Supports storage autoscaling and vertical scaling by changing the RDS instance type e.g. XL -> 2XL.
- Storage Autoscaling and EBS-backed storage (GP2, GP3, IO-Optimized).
- Integrated security features: encryption (KMS), IAM authentication, backups stored in S3, and VPC network isolation.

---

## AWS Databases – Exam Essentials

- Amazon RDS is a fully managed relational database service where provisioning, patching, backups, and Multi-AZ high availability are handled by AWS.
- Amazon RDS offers features like Read Replicas, RDS custom, Storage autoscaling and Automated/manual snapshots.
- Amazon Aurora is a MySQL/PostgreSQL-compatible AWS-optimized relational database that offers high performance at 1/10th cost. Amazon Aurora offers features like Global database, DB cloning and Machine Learning.
- Features like RDS proxy and Reserved Instances work with both Amazon RDS and Aurora databases.
- Amazon ElastiCache is an in-memory cache (Redis/Memcached/Valkey) used to offload reads from the databases.
- Amazon DocumentDB is a JSON document database compatible with MongoDB APIs. Used for content management.
- Amazon Neptune is a graph database for highly connected data such as social networks, fraud detection, and knowledge graphs.
- Amazon Timestream is a serverless time-series database optimized for IoT metrics, application monitoring, and time-stamped data.
- Amazon QLDB is an immutable, cryptographically verifiable ledger database for audit and tracking systems.
- Amazon Keyspaces is a fully managed Cassandra-compatible wide-column database for scalable, low-latency workloads.

### Amazon DynamoDB
- Amazon DynamoDB is a fully managed, serverless cloud-native **NoSQL** database service that provides millisecond latency at any scale.
- Has Partition key, Sort key (optional) and attributes. Partition key + Sort key = Primary Key.
- Query and Scan operations – Query fetches specific item, Scan fetches all items in the table. Optionally use GSI.
- Supports Standard Table class and Infrequent Access (IA) Table class.
- Supports Provisioned Capacity mode (default) and On-demand capacity mode (RCU/WCU).
- Supports Global tables (Active-Active) for global applications.
- Supports DynamoDB accelerator (DAX) providing microsecond latency.
- Supports Streams to act on table level modifications. Integrate with other AWS services like Lambda, SQS, SNS, Kinesis for stream processing.
- Supports Time-To-Live (TTL) to automatically expire and delete the items from the table.
- Supports PITR and On-demand backups. PITR allows restoring a new table (up to 35 days).
- Supports Exporting data to S3 and importing data from S3.

---

## AWS Databases - Exam Scenarios

### 1. Scenario
A global e-commerce company wants an active-active database across multiple regions so customers can place orders from anywhere with low latency.

**Key-words:** Active-Active, multiple regions, place orders, from anywhere, low latency

**Answer:** DynamoDB Global Tables

*Why not Amazon Aurora Global Database → Place orders is a write operation and Aurora supports Read-replicas across the regions.*

### 2. Scenario
A ride-sharing app needs to cache frequently accessed data like driver locations to reduce database load and achieve microsecond latency.

**Key-words:** Cache

**Answer:** ElastiCache or DynamoDB Accelerator (DAX)

*It could be either of ElastiCache and DAX and if answer just contains one of them then it's an easy pick. However, if both the options are there, then look for additional information e.g. source database is DynamoDB then it will be DAX or if the source database is RDS/Aurora or it mentions about advanced features then it will be ElastiCache.*

### 3. Scenario
A streaming platform wants to store large volumes of user activity logs but only needs them for occasional analytics queries.

**Key-words:** occasional, analytics queries

**Answer:** DynamoDB IA Table class or DynamoDB export to S3

*Depending on additional context, if it's about saving the storage cost then answer will be DynamoDB Infrequent Access Table class or if it's about analyzing the data using Athena or other tools then answer will be exporting the data to S3.*

### 4. Scenario
A ride-sharing platform wants to quickly determine the shortest path between drivers and riders based on a complex graph of roads and connections.

**Key-words:** shortest path, graph, connections

**Answer:** Amazon Neptune

### 5. Scenario
A team needs to quickly create a full copy of their production database to run performance tests and simulate heavy workloads without affecting the live system. The copy should be created within minutes.

**Key-words:** full copy of production, within minutes

**Answer:** Aurora Cloning

*You may think of Read replicas but the scenario doesn't mention about only reading the data. Further, Aurora's decoupled and shared storage makes the cloning really fast (copy-on-write) and hence the time to create database clone is faster than other possible options.*

### 6. Scenario
An organization wants to migrate their on-premises Oracle database to Amazon Aurora PostgreSQL. The schema structures between the two engines differ, and they need help automatically converting tables, stored procedures, functions, and views. After conversion, they want to perform a continuous replication until cutover.

**Key-words:** migration, Oracle to PostgreSQL, schema differs

**Answer:** Amazon DMS with SCT

*Amazon Database Migration Service (DMS) for the database migration and Schema-conversion Tool for converting the schema from Oracle to PostgreSQL engine.*

---

## Big Data Analytics Services – Exam Essentials

### AWS Glue
- Serverless ETL and data integration service built on Apache Spark.
- Uses Glue Crawlers to discover schemas and populate the Glue Data Catalog.
- Performs batch and streaming ETL using Glue Jobs (Spark / Ray / Python Shell).
- Job Bookmarks enable incremental processing and prevent reprocessing.
- Glue Studio provides a visual, low-code way to build ETL pipelines.
- Supports encryption at rest and in transit using AWS KMS.

### Amazon EMR
- Managed service for ETL and large-scale big data processing, offering more flexibility than AWS Glue.
- Supports Hadoop, Spark, Hive, Flink, and other big data frameworks.
- Runs on EC2 (including EC2 Spot instances) for cost optimization.
- Can be integrated with ECS and EKS for container-based big data workloads.
- Uses HDFS backed by EBS / instance store for high-performance, temporary storage.
- Uses EMRFS to store and access data directly in Amazon S3 for durable storage.

### Amazon Athena
- Serverless service to query and analyze data in **Amazon S3 using SQL**.
- Uses the AWS Glue Data Catalog for table definitions and schema metadata.
- Pay-per-query pricing based on data scanned, not query execution time.
- Query cost can be reduced by partitioning data in S3 (prefixes) based on fields in WHERE clauses.
- Supports Federated Query to query non-S3 data sources using Lambda-based connectors.
- Can be used to analyze AWS service logs such as CloudTrail, ALB access logs, and VPC Flow Logs.

### Amazon QuickSight
- Serverless business intelligence (BI) service used to create interactive dashboards and visualizations.
- Uses SPICE (in-memory engine) for fast query performance and reduced load on data sources.
- Has its own users and groups (separate from IAM); groups are available in Enterprise edition.
- Dashboards are read-only snapshots of analyses and must be published before sharing.
- Supports secure sharing and embedding of dashboards into applications.
- Supports Row-Level Security (RLS) to restrict which data rows users can see (Enterprise edition).

### Amazon Redshift
- Fully managed data warehouse designed for fast analytics on structured data using SQL.
- Supports Provisioned clusters (manual sizing) and Redshift Serverless (auto-scaling, pay per RPU).
- Uses leader node (query planning) and compute nodes (data storage and execution) in provisioned mode.
- Supports Zero-ETL integrations to analyze data directly from services like Aurora and DynamoDB.
- Redshift Spectrum allows querying data directly in Amazon S3 without loading it into Redshift.
- Data is commonly loaded using the COPY command from S3 (parallel, high performance).
- Auto COPY automatically loads new files from S3 into Redshift as data arrives.

### AWS Lake Formation
- Service for centralized data lake governance (not an analytics or ETL engine).
- Provides fine-grained access control at table, column, and row level, built on top of the AWS Glue Data Catalog.
- Supports LF-tags for scalable, attribute-based access control (e.g., department=finance).
- When enabled, replaces direct IAM + S3 permissions for Athena, Redshift Spectrum, and EMR access.
- Can optionally bootstrap data lakes using workflows and blueprints during initial setup.

### Amazon Kinesis Data Stream
- Managed service to ingest and store real-time streaming data (logs, events, metrics) for downstream processing.
- Data is stored in shards, each providing 1 MB/sec write (1,000 records/sec) and 2 MB/sec read throughput.
- Supports Provisioned mode (manual shard management) and On-Demand mode (automatic scaling based on recent traffic patterns).
- Data retention is 24 hours by default, configurable up to 365 days, enabling replay and reprocessing.
- Records are immutable once written and cannot be deleted individually.
- Common use cases include log ingestion, clickstream analysis, IoT telemetry, and real-time monitoring.

### Amazon Kinesis Video Stream
- Secure ingestion and processing of live video and media streams from devices and cameras.
- Used for driver behavior analysis, motion detection, security surveillance, and smart home / industrial video analytics.

### Amazon Managed Service for Apache Kafka (MSK)
- Fully managed Kafka service that lets you run Kafka-compatible streaming applications without managing brokers.
- Used when Kafka APIs, tools, or ecosystem compatibility is required for real-time data ingestion.

### Amazon Managed Service for Apache Flink (MSF)
- Fully managed service for real-time stream processing, not just ingestion or delivery.
- Used for stateful processing, windowed aggregations, and event-time–based analytics.
- Commonly consumes data from Kinesis Data Streams or Amazon MSK.
- You explicitly define output sinks (e.g., S3, DynamoDB, Redshift, OpenSearch).
- Ideal for complex streaming use cases such as fraud detection, anomaly detection, and real-time metrics.

### Amazon Data Firehose
- Fully managed service to deliver streaming data to destinations with no consumer or shard management.
- Supports delivery to Amazon S3, Amazon Redshift, Amazon OpenSearch Service, and HTTP endpoints.
- Automatically scales and is serverless; you pay for data processed.
- Delivers data in near real time using configurable buffer size and buffer interval.
- Supports data format conversion (e.g., JSON → Parquet/ORC) and compression.
- Allows optional AWS Lambda transformations for simple enrichment or filtering.

---

## AWS Analytics Services – Match the Pairs

| Scenario | Matching Service |
|---|---|
| You need to process very large datasets using Spark or Hadoop | Amazon EMR |
| Migrate existing on-premises Hadoop workloads to AWS | Amazon EMR |
| Real-time processing on streaming data in event-time window | Amazon Managed Service for Apache Flink (MSF) |
| Transform and deliver streaming data to S3/Redshift/OpenSearch | Amazon Data Firehose |
| Minimal operational overhead to extract, transform, and load data into a data lake or data warehouse | AWS Glue |
| Centrally manage metadata and make datasets discoverable | AWS Glue (Data Catalog) |
| Run SQL queries directly on data stored in Amazon S3 | Amazon Athena |
| Business users need interactive dashboards and visual analytics | Amazon QuickSight |
| Ingest and process open-source Kafka-compatible streaming data | Amazon Managed Service for Apache Kafka (MSK) |
| Granular level permissions at database, table, column or row level | AWS Lake Formation |
| Data warehouse for fast analytics on structured data at scale | Amazon Redshift |
| Ingest high-throughput streaming data from mobile/web apps | Kinesis Data Stream |

---

## AWS Machine Learning and AI – Exam Essentials

- Amazon's three-layered approach to AI/ML – Infrastructure, ML platform, AI services.
- **Amazon Rekognition** – Computer vision-based object detection (Face, celebrity, text, scene) in Images and Videos.
- **Amazon Transcribe** - Speech to text.
- **Amazon Polly** - Text to Speech.
- **Amazon Textract** – Extract text from scanned documents.
- **Amazon Translate** – Translation from one language to other.
- **Amazon Comprehend** – Entity extraction, sentiment analysis.
- **Amazon Kendra** – Enterprise search engine for employees and customers.
- **Amazon Connect** - Cloud contact center.
- **Amazon SageMaker AI** - Machine learning platform for developers and data scientists.

### Amazon SageMaker AI
- Fully managed service for developers / data scientists to build, train and deploy ML models at scale.

**Amazon SageMaker AI Features:**
- **SageMaker Studio** – A web-based IDE interface for building, training and deploying models.
- **AutoML (SageMaker Autopilot)** - Automatically explores and creates the best ML models.
- **Built-in Algorithms** – A wide range of built-in machine learning algorithms optimized for performance and scalability.
- **Notebook Instances** - Managed Jupyter notebooks with pre-installed libraries, scalable compute, and data storage, making it easier to explore data and develop models.
- **Training & Tuning** – Manages ML training infrastructure, Distributed training across multiple GPUs.
- **Model deployment** – Deploy models to production in one click by creating endpoints, multi-model endpoints.
- **SageMaker GroundTruth** – A data labelling service (Text, images, video labelling).
- **SageMaker Model Monitor** - Continuously monitors the quality of your models in production, detecting drift in model performance.
- **SageMaker Pipelines:** Facilitates the automation and orchestration of machine learning workflows, supporting the implementation of MLOps best practices.

---

## AWS Edge Networking – Exam Essentials

- AWS provides a global edge network with hundreds of edge locations and regional edge caches to improve latency and performance.
- These edge locations are used by Amazon CloudFront, AWS Global Accelerator and S3 Transfer Acceleration.
- Amazon CloudFront delivers content through globally distributed edge locations with regional edge caches.
- Amazon CloudFront supports multiple origins including S3, API Gateway, ALB, EC2, and custom HTTP endpoints.
- CloudFront enables multiple origins with path-based routing using cache behaviors.
- For S3 origins, use Origin Access Control (OAC) to keep the bucket private and allow only CloudFront to access objects.
- CloudFront security features include HTTPS, mTLS, Field-Level Encryption, Signed Cookies, and Signed URLs for secure content delivery.
- Private content can be enforced using OAC for S3 or a custom header validated by origins like ALB or API Gateway.
- For VPC-based origins, restrict access so only CloudFront can reach them using Security Groups and either ip-ranges.json (CloudFront CIDRs) or the CloudFront managed prefix list (recommended).
- CloudFront includes features like Geo Restriction, Cache TTL control, and Cache invalidations for content freshness and access control.
- Serverless edge compute is available through CloudFront Functions (ultra-light, JS, <1 ms) and Lambda@Edge (heavier, Python/Node, for advanced logic).
- Additional security can be added via AWS WAF for layer-7 protection and AWS Shield for DDoS protection.
- CloudFront supports Alternate Domain Names (CNAMEs) and integrates with Route 53 for DNS-based routing.
- AWS Global Accelerator provides global TCP/UDP acceleration with two static Anycast IPs, routing users to the nearest healthy AWS endpoint enabling region level failover and IP whitelisting capabilities.

> - Global Content delivery + Caching → **CloudFront**
> - TCP/UDP traffic + Static IPs + Health checks + Region Failover → **AWS Global Accelerator**

---

## Route 53 (DNS) – Exam Essentials

- Route 53 is AWS's highly available and scalable DNS and domain management service.
- Route 53 Hosted Zone is a container for DNS records for a domain; public hosted zones handle internet-facing domains, while private hosted zones work only inside one or more VPCs.
- A records map a domain name to an IPv4 address.
- AAAA records map a domain name to an IPv6 address.
- CNAME records map a DNS name to another DNS name but cannot be used at the zone apex.
- Alias records are Route 53–specific and map a name to AWS resources like CloudFront, ALB, API Gateway, or S3, and can be used at the zone apex.
- Alias records automatically track IP changes of AWS resources and do not incur DNS query charges for AWS targets.
- Route 53 Health checks monitor endpoints (HTTP, HTTPS, TCP) and can be associated with DNS records to remove unhealthy endpoints from responses.
- Health checks support CloudWatch alarms for additional monitoring flexibility.
- TTL (Time to Live) defines how long DNS resolvers cache responses; lower TTLs reduce caching time but increase DNS traffic.

### Routing Policies
- **Simple routing** returns all IPs in a single record (or a single value set) without health checks.
- **Weighted routing** distributes traffic across multiple records based on assigned weights for A/B testing or traffic shifting.
- **Latency routing** directs clients to the AWS region with the lowest measured latency for improved performance.
- **Failover routing** supports active–passive architectures by using health checks to switch between primary and secondary.
- **Geolocation routing** maps user locations (continent, country, or state) to specific endpoints for location-based content delivery or compliance.
- **Geoproximity routing** directs traffic based on geographic distance between users and endpoints, with optional bias (+/- 1-99) to shift traffic toward or away from regions; requires Traffic Flow.
- **Multi-Value Answer routing** returns up to 8 healthy IPs and provides basic DNS-level load balancing with health checks.
- **IP-based routing** routes queries based on the client's IP address range for fine-grained traffic control.

### Route 53 Resolver
- Route 53 Resolver endpoints enable hybrid DNS between on-premises and AWS environments.
- Inbound resolver endpoints allow on-prem systems to resolve private DNS names inside AWS VPCs.
- Outbound resolver endpoints enable VPC resources to query on-prem DNS servers.
- Resolver endpoints use ENIs in two subnets for high availability and support cross-account sharing via AWS RAM.

---

## Serverless: Amazon API Gateway – Exam Essentials

- API Gateway supports REST, HTTP, and WebSocket APIs, enabling traditional request/response, lightweight microservices, and real-time bidirectional communication.
- APIs method integrations – Lambda, HTTP/proxy, AWS services, Mock and VPC Link.
- API Gateway offers edge-optimized, regional, and private endpoints to match global performance, regional access, or VPC-only connectivity needs.
- API Gateway enforces HTTPS for all endpoints, and edge-optimized custom domains require the ACM TLS certificate to be created in the us-east-1 (N. Virginia) region.
- VPC Link v2 enables secure, high-performance private connectivity from API Gateway to services inside your VPC using ALBs without exposing them publicly. VPC Link V1 supports NLB.
- API Gateway supports IAM, Cognito User Pools, and Lambda Authorizers, allowing granular security through signatures, JWT tokens, or custom auth logic.
- You can map your APIs to custom domain names with ACM certificates.
- API Gateway provides versioning, stages/environments, API keys with usage plans, caching, request/response transformations, and WAF integration to improve manageability, performance, and security.

---

## Serverless: AWS Lambda – Exam Essentials

- Lambda is a Serverless compute ideal for stateless executions which needs to run for short amount of time (& vanish) e.g. API backend, Event processing, Data processing etc.
- **Versioning:** Every publish creates an immutable version of your function code + configuration.
- **Aliases:** Named pointers to versions (e.g., dev, prod), supporting zero-downtime deployments and weighted routing.
- **Concurrency:** Number of executions running at once; limited by account concurrency quotas (default = 1000).
- **Reserved Concurrency:** Reserves capacity for a function and prevents others from using it thereby prevents function throttling.
- **Lambda Cold Start:** One-time initialization delay when Lambda creates a new execution environment for a request.
- **Provisioned Concurrency:** Pre-initializes execution environments to eliminate cold starts for predictable, latency-sensitive workloads.
- **Synchronous Invocation:** Caller waits for response (API Gateway, ALB); throttles return 429 errors immediately.
- **Asynchronous Invocation:** Lambda queues events internally, retries automatically on failure, and can forward to DLQ or failure-on destinations. For failures due to throttling or system errors, requests are retried for up to 6 hours.
- **Lambda SnapStart:** Creates and restores from a runtime snapshot to drastically reduce cold start times.
- **Monitoring:** Lambda sends logs and metrics such as duration, errors, and throttles to CloudWatch automatically.
- **Lambda in VPC:** Enables connecting to resources in VPC private subnets e.g. RDS, EC2, ElastiCache etc.

---

## SQS, SNS, EventBridge – Exam Essentials

### Amazon SQS
- SQS enables asynchronous communication and decouples producers from consumers.
- Standard queues provide best-effort ordering and at-least-once delivery.
- FIFO queues guarantee strict ordering and exactly-once delivery.
- Visibility timeout prevents duplicated processing. If application needs more time to process the message, it can update the visibility timeout using `ChangeMessageVisibility` API.
- Delay queues postpone message delivery for up to 15 minutes.
- Batch operations reduce API calls, maximum 10 messages per batch for normal API operation however Lambda can process up to 10,000 messages per batch.
- Long polling reduces empty receives and lowers cost.
- Dead Letter Queue (DLQ) isolates repeatedly failing messages for debugging.
- SQS supports encryption at rest and in transit, and queue policies enable access for IAM identities to send or receive messages from the queue including cross-account access.

### Amazon SNS
- SNS is a pub/sub system that broadcasts messages to multiple subscribers.
- SNS supports email, SMS, HTTP, Lambda, and SQS deliveries.
- SNS FIFO maintains ordering and exactly-once semantics.
- SNS with SQS (fan-out) lets multiple microservices receive the same event independently.

### Amazon EventBridge
- EventBridge routes events between AWS services, SaaS apps, and custom apps.
- Rules match events based on schedules or event patterns.
- Advanced filtering includes prefix, numeric, anything-but, exists, and CIDR matching.
- Rules can have multiple targets, and unmatched events are dropped unless a catch-all rule is created.
- Event archiving stores events for long-term retention and replay.
- Schema registry discovers, stores, versions, and generates code bindings for event schemas.

---

## ACM – Exam Essentials

- ACM provides, stores, and auto-renews public and private SSL/TLS certificates for AWS services.
- ACM public certificates are free when used with AWS services (ALB, NLB, CloudFront, API Gateway, App Runner, etc.).
- Wildcard certificates are supported (e.g., `*.example.com`).
- Exportable public certificates are optional and incur cost.
- For auto-renewal of Public Certificates, DNS validation method is preferred.
- Imported certificates do NOT auto-renew — you must re-import before expiry.
- ACM certificates must be issued in the same region as the service, except CloudFront (always us-east-1).
- EventBridge automatically receives ACM certificate lifecycle events (expiry, renewal failure, etc.).
- Route 53 Alias records are used to map custom domain names to ELB/CloudFront along with ACM certs.
- ACM integrates with AWS Private CA (PCA) to issue private certificates for internal workloads (ACM itself cannot issue private certs without Private CA backing it).

> **Exam Tip:** ACM = Public Certificates for AWS services, Auto-renewal, Free; PCA = Private Certificates

---

## KMS – Exam Essentials

- KMS is a key management service used to create, store, and control cryptographic keys.
- Supports both symmetric keys (encrypt/decrypt data) and asymmetric keys (for sign/verify).
- Keys never leave KMS — operations happen inside AWS-managed HSMs (FIPS 140-2 validated).
- IAM permissions + Key Policy together control access (like S3 bucket policies).
- KMS keys are regional, unless created as multi-region keys for cross-region DR or replication.
- Envelope Encryption is used by most AWS services (S3, EBS, RDS, DynamoDB).
- Multi-Region Keys support DR and global applications and used for AMI copy, S3 CRR, DynamoDB Global Tables, Secrets Manager multi-region.
- You can share KMS keys across AWS accounts by updating the key policy.
- KMS logs every key usage in CloudTrail — critical for security/compliance.
- Automatic key rotation is supported for symmetric customer-managed keys (annual rotation).
- KMS integrates with most of AWS services handling data, making encryption at rest easy and consistent.

---

## Secrets Manager – Exam Essentials

- Securely stores sensitive credentials such as passwords, API keys, and tokens, no need to hardcode them in applications.
- Supports automatic rotation for integrated services (RDS/Aurora) and custom rotation via Lambda for others.
- Secrets are encrypted with KMS and access is controlled using IAM permissions.
- Supports multi-region secret replication for DR and low-latency access; rotation occurs in the primary region only.
- You can promote a replica to primary during regional failover to continue updates/rotation.
- All secret access, updates, and rotations are logged in CloudTrail for auditability.
- Designed specifically for high-security secrets, unlike SSM Parameter Store, which does not offer rotation or native multi-region sync.
- Works with many AWS services (Lambda, EC2, ECS, EKS, RDS) for seamless credential management.

---

## AWS Infrastructure Deployment & DevOps

### Infrastructure Deployment
- **AWS CloudFormation** – Infrastructure as a code (YAML or JSON templates).
- **Cloud Development Kit (CDK)** - Create, share, deploy CloudFormation template programmatically.

### DevOps
- **CodeCommit** – Git compatible source code depository.
- **CodeBuild** – Compile source code and run tests.
- **CodeDeploy** – Deploy s/w builds onto the compute servers (EC2, on-premises, ECS, Lambda).
- **CodePipeline** – End to end CI/CD pipeline automation.
- **CodeArtifact** – Software repository / package management.

### Application Deployment Services
- **Elastic Beanstalk** – Fully managed platform-as-a-service (PaaS) for 3-tier web applications (ALB, ASG, EC2).
- **Lightsail** – Preconfigured servers with fix capacity and price for simple applications, databases and containers.
- **Amplify** – Fully managed frontend platform with built-in CI/CD and backend integrations.

### AWS CloudFormation – Details
- AWS-native Infrastructure as Code service to provision and manage AWS resources consistently.
- Uses JSON/YAML template to define the desired state of infrastructure.
- Template contains different sections like Resources (mandatory), Parameters, Mappings, Conditions, Outputs etc.
- Nested stacks allow large templates to be split into smaller, reusable components managed by parent template.
- StackSets enable deploying the same CloudFormation template across multiple AWS accounts and Regions.
- StackSets are commonly used with AWS Organizations for centralized governance.
- Change sets let you preview which resources will be added, modified, or deleted before executing the changes.
- Drift detection identifies changes made outside CloudFormation by comparing actual resources with the template.
- CloudFormation automatically rolls back failed stack creations or updates to the last stable state.
- DeletionPolicy controls resource behavior during rollback or stack deletion. Supports Retain, Snapshot & Delete actions.
- CloudFormation can use an IAM Service Role to create/update/delete resources.
- IAM user/role should have `iam:PassRole` permissions to pass Service Role to CloudFormation.

---

## AWS Elastic Beanstalk – Exam Essentials

- Platform as a Service (PaaS): You upload code, Beanstalk provisions and manages infrastructure.
- Handles EC2, Auto Scaling, Load Balancer, health checks, and monitoring using CloudWatch.
- Supports Web (HTTP/HTTPS) and Worker environments (SQS-based background processing).
- Single-instance vs load-balanced option exists for web environments.
- Worker environments always use Amazon SQS and Auto Scaling.
- Does not manage databases; databases are external services (RDS, DynamoDB, etc.).
- Environment configuration is versioned and supports rollback.
- Multiple deployment strategies: All at once, Rolling, Rolling with additional batch, Immutable, Traffic splitting, Blue/green.
- Immutable deployments are safest; All at once causes downtime, Blue/green deployments are done by creating a separate environment and swapping CNAMEs/URLs.
- Amazon Lightsail provides Virtual Private Server (VPS) with pre-bundled app-blueprints e.g. wordpress, Node.js, Drupal etc.

---

## Logging and Monitoring – Exam Essentials

### Amazon CloudWatch
- Amazon CloudWatch is the primary monitoring and observability service in AWS which is used to collect, visualize, and act on metrics, logs, and events.
- CloudWatch metrics are time-series data published under AWS/ or custom namespaces.
- Metric streams continuously stream metrics to external destinations with namespace-level filtering only.
- CloudWatch alarms monitor metrics and can trigger notifications or automated actions.
- A CloudWatch composite alarm triggers actions based on the combined state of multiple alarms.
- CloudWatch Logs store application and service logs organized into log groups and log streams.
- Logs can be queried using CloudWatch Logs Insights or streamed using subscription filters.
- Subscription filters support Lambda, Kinesis Data Streams, and Kinesis Data Firehose destinations.
- CloudWatch Agent is used to collect system-level metrics and logs from EC2 and on-premises servers.
- CloudWatch Insights provide deeper visibility into Containers, Lambda, Database, and Application related performance metrics. Additionally provides Contributor insights to identify top contributors for a given metric.
- CloudWatch Dashboards provide visual monitoring views and can be shared securely by email or publicly.

### Amazon X-Ray
- AWS X-Ray is used for distributed tracing of applications.
- It helps identify latency, errors, and request paths across services.
- X-Ray provides service maps and requires application instrumentation.

### AWS Health
- AWS Health Dashboard is used to monitor AWS service and account health.
- It shows outages, maintenance, and account-specific events.
- Health events can be integrated with EventBridge for automation.

---

## AWS Systems Manager – Exam Essentials

- AWS Systems Manager provides operational visibility and automation for managing AWS and hybrid resources like EC2 instances, on-premises servers, and VMs using the SSM Agent.
- Run Command executes commands on instances using Command-type SSM documents.
- Session Manager enables secure, audited access over browser or CLI without security group inbound ports or SSH key pairs. It uses IAM permissions for the access.
- Patch Manager automates OS patching using patch baselines and maintenance windows.
- State Manager ensures instances remain in a desired configuration state.
- Parameter Store stores configuration values in hierarchical format. Access is protected by IAM permissions and values can be optionally encrypted using KMS.
- SSM Documents define operational actions and can be AWS-managed or custom.
- Automation documents enable multi-step workflows with branching, waiting, and approvals.
- Automation can be invoked manually, on a schedule (Maintenance Windows), via State Manager associations, or event-driven using EventBridge or AWS Config remediation.

---

## AWS CloudTrail – Exam Essentials

- CloudTrail answers **"who did what, when, and from where"** in an AWS account.
- CloudTrail records API activity and account actions across AWS services.
- Management events record control-plane actions and are enabled by default.
- Data events record data-plane actions (for example, S3 object access) and are not enabled by default.
- CloudTrail Event History is available for 90 days without creating a trail.
- A CloudTrail trail is required for long-term storage, delivery to S3 or CloudWatch Logs, and compliance.
- Trails can be single-region or multi-region.
- CloudTrail Insights detects unusual or anomalous API activity using ML.
- CloudTrail Lake provides SQL (Trino-based) querying over CloudTrail events.

---

## AWS Config – Exam Essentials

- AWS Config is a service that records and tracks the configuration state of AWS resources over time.
- A Configuration Recorder must be enabled to capture resource configurations and changes.
- Config Rules evaluate resources against desired configuration conditions.
- Rules mark resources as compliant or non-compliant.
- AWS provides managed rules and supports custom rules using Lambda.
- Config Rules run in a single account and single region.
- Configuration Aggregators provide a centralized, read-only view across multiple accounts and regions.
- Aggregators do not evaluate resources or trigger actions.
- AWS Config supports notifications via SNS when compliance changes.
- Remediation actions can be configured using SSM Automation documents.

---

## AWS Security Services – Exam Essentials

- **AWS WAF** - Protects your web applications from common web exploits (Layer 7).
- **AWS Shield and Shield Advanced** - Maximize application availability and responsiveness with managed DDoS protection (Layer 3/4).
- **AWS Network Firewall** - A stateful intrusion detection and prevention service (IPS/IDS).
- **AWS Firewall Manager** - Centrally configure and manage Firewall rules across AWS accounts.
- **Amazon Inspector** - Find software vulnerabilities in EC2, ECR Images, and Lambda functions.
- **Amazon GuardDuty** - Protect AWS accounts with intelligent threat detection by analyzing AWS services logs (DNS logs, VPC flow logs, CloudTrail event logs).
- **AWS Security Hub** - Centrally gather security findings and security alerts from multiple AWS accounts.
- **Amazon Detective** - Find the root cause of security issues or suspicious activities.

---

## Storage and Data Migration – Exam Essentials

### File Storage - EFS
- Amazon EFS is a fully managed NFS file system that can be mounted by multiple EC2 instances or containers across multiple Availability Zones.
- Automatically scales storage and is commonly used for shared file systems in Linux-based workloads.
- Works well for containerized application as persistent file system.

### File Storage - FSx
- FSx for Windows File Server provides a managed Windows file system using SMB with native Active Directory integration.
- Used for Windows applications that require shared file storage and NTFS features.
- FSx for Lustre is a high-performance parallel file system used for HPC and ML workloads for fast, concurrent access.
- FSx for Lustre integrates with Amazon S3 for processing large datasets.

### Offline Data Transfer - Snow Family Devices *(Service discontinued)*
- AWS Snow Family is used to transfer very large amounts of data when network transfer is slow, unreliable, or not available.
- Offers AWS Snowcone device (portable 8-14TB storage) and AWS Snowball edge device (80-210TB storage).
- Commonly used for data center migrations, disaster recovery, and edge computing scenarios.

### Online Data Transfer – AWS DataSync
- Used for fast, automated online data transfer between on-premises storage and AWS and between AWS services.
- Best suited for one-time or scheduled migration of data to Amazon S3, EFS, or FSx.
- Offers features like bandwidth limit, parallel tasks, retry, handling network failure etc.

### Online Data Transfer – AWS Transfer Family
- AWS Transfer Family provides managed SFTP, FTPS, FTP, and AS2 access to data stored in Amazon S3 or EFS.
- Used for partner, customer, or legacy system file transfers without managing FTP servers.

### Hybrid Storage – AWS Storage Gateway
- AWS Storage Gateway enables hybrid storage by connecting on-premises environments to AWS storage services.
- Supports file gateway (S3 File gateway and FSx File gateway), volume gateway, and tape gateway interfaces backed by Amazon S3 or Amazon FSx.

### Backup – AWS Backup
- AWS Backup is a centralized service to automate and manage backups across multiple AWS services and 3rd party applications.
- Used for policy-based backups with retention, lifecycle, and cross-region, cross-account support.

---

## AWS Storage Services – Match the Pairs

| Scenario | Matching Service | Type |
|---|---|---|
| Processing large scientific datasets by multiple compute nodes | FSx for Lustre | Filesystem |
| Social Media Images accessible to Linux EC2 instances for processing | EFS | Filesystem |
| Social Media Images accessible to users | S3 | Bucket |
| Hosting static website | S3 | Bucket |
| Persistent filesystem for containers | EFS | Filesystem |
| Hosting an OS for an EC2 instance | EBS | Volume |
| Storage for a relational database | EBS | Volume |
| Storing backups and archives | S3 | Bucket |
| Temporary high-throughput storage integrated with S3 | FSx for Lustre | Filesystem |
| Data lake storage for analytics and ML workloads | S3 | Bucket |
| Shared storage for Microsoft workloads | FSx for Windows | Filesystem |

---

## Migration and Disaster Recovery - Exam Essentials

### Migration
- There are different AWS services for supporting large scale migration. This includes Migration Evaluator, Migration Hub, Application Discovery Service, Application Migration Service, AWS Database Migration Service etc.
- From Nov'2025, AWS has discontinued most of the migration services and launched AWS Transform service as one stop AI based service for all types of Migrations.

### Application Migration Service (MGN)
- Used for lift-and-shift (rehost) migration of servers to AWS.
- Replication Agent must be installed on each source server (on-prem or other cloud).
- Performs continuous block-level replication of server disks.
- Uses an AWS-managed EC2 + EBS staging area and launches EC2 instances during Test and Cutover phases with no/minimal downtime.

### AWS Database Migration Service (DMS)
- Used for database migration, supporting like-to-like and heterogeneous migrations.
- Uses AWS SCT (Schema conversion Tool) for heterogeneous migrations (e.g. Oracle to Aurora PostgreSQL).
- Uses an EC2-based replication instance. Deploy replication instance in Multi-AZ for high availability.
- For high transactional (read/write) CDC, use memory-optimized instances and for complex transformations or many parallel tasks, use compute-optimized instances.

### Disaster Recovery
- **RPO:** Maximum acceptable data loss (in time), **RTO:** Maximum acceptable downtime.

### DR Strategies
- **Backup & Restore** = Backups stored in Amazon S3 / Glacier, Lowest cost, highest RTO.
- **Pilot Light** = Minimal core resources running in AWS.
- **Warm Standby** = Scaled-down but fully functional environment.
- **Multi-Site / Active-Active** = Fully running in multiple Regions, Lowest RTO/RPO, highest cost.

### Common AWS Services Used for DR
- Amazon Machine Image (AMI) – Pre-configured OS/Apps for faster deployment.
- EBS snapshots – Backup of EBS volumes for restore.
- AWS CloudFormation – To provision infrastructure in the recovery region.
- Amazon S3 and S3 Glacier - Backup storage and long-term retention.
- AWS Storage Gateway - For backup of data from on-premises to AWS (S3, EBS snapshots).
- Amazon Route 53 - DNS-based failover and traffic routing for active-active DR.
- Aurora Global Database and DynamoDB Global Tables for active-active databases for active-active DR.

---

## AWS Multi-account Management – Exam Essentials

### AWS Organization
- AWS Organizations is used for centralized management of multiple AWS accounts.
- Accounts are organized using Organizational Units (OUs) in a hierarchy (up to 5 levels).
- Supports two modes: Consolidated billing only and All features enabled.
- All features enabled is required for SCPs, Tag Policies, and Backup Policies.
- Consolidated billing provides single bill, volume discounts, and shared RI/Savings Plans.
- Service Control Policies (SCPs) define the maximum allowed permissions for accounts.
- SCPs do not grant permissions. Effective permissions are the intersection of SCPs from root to the account.
- SCPs affect all IAM users and roles, including root (except users in management account).
- SCP can be used for restricting access for AWS services, specific AWS regions, enforcing S3 encryptions etc.
- Tag Policies standardize tag keys, values, and case across accounts.
- Tag policies can be applied in enforced mode or monitoring mode.
- Use Tag policies and SCP together to control tag-based access for AWS resources.
- IAM Global condition key `aws:PrincipalOrgID` is used in resource policies to allow access only from accounts in the given AWS organization.

### AWS Control Tower
- AWS Control Tower is used to set up and govern a secure multi-account AWS environment.
- Built on top of AWS Organizations and requires all features enabled.
- By default creates Security OU under which it creates Logging Account and Audit Account.
- Enforces governance using Preventive and Detective guardrails.
- Provides Account Factory to create new accounts in a standardized way.
- Uses AWS CloudFormation and Service Catalog to provision AWS resources in the accounts.
- Integrates with IAM Identity Center for centralized access management.

### AWS Resource Access Manager (RAM)
- AWS Resource Access Manager enables secure sharing of AWS resources across accounts.
- Eliminates the need to duplicate resources in each account.
- Supports sharing with Accounts in the same organization and External AWS accounts (invite-based).
- Shared resources remain owned and billed to the resource owner account.
- Commonly shared resources include: VPC subnets, AWS Transit Gateway, Cloud HSM etc.

### IAM Identity Center
- AWS IAM Identity Center (formerly AWS SSO) provides centralized SSO across multiple AWS accounts.
- Integrates natively with AWS Organizations.
- Replaces long-term IAM users with federated, short-term access.
- Uses permission sets to define access (translated into IAM roles per account).
- Permission sets can be assigned to users or groups (groups preferred).
- Supports only one identity source at a time.

**Identity sources include:**
- Identity Center directory (Built-in)
- AWS Managed Microsoft AD
- On-premises Microsoft AD (Two-way Trust)
- AD Connector (proxy to on-prem Microsoft AD)
- External SAML 2.0 IdP

- AWS Managed Microsoft AD is used when AWS workloads need full AD features.

---

## AWS Billing and Cost Management – Exam Essentials

- **AWS Pricing calculator** – Create cost estimates and share with others (free tool).
- **AWS Bills** – Overall monthly bills with service level breakup.
- **Cost Explorer** – Detailed cost breakup and analysis and also cost forecast.
- **Cost Anomaly detector** – Detect unusual spend using historic usage data and machine learning.
- **Data Export** – Raw dataset for creating your own analysis and dashboard (Formerly CUR).
- **AWS Billing Alert** – CloudWatch alarm in us-east-1 (N. Virginia) region to alert when AWS usage exceeds.
- **AWS Budgets** – Create budgets for overall AWS usage or service usage or RI/Savings plan usage.
- **Cost Allocation Tags** – Group the AWS resources by resource tags and create cost categories.
- **Cost Optimization Hub** – Recommendation for purchasing Savings Plan, Reserved Instances.
- **AWS Cost Optimizer** – Recommendations to optimize cost by right sizing resources (e.g. EC2 size).
- **AWS Free tier dashboard** – Free trials and Always free services.

---

*All rights reserved © www.awswithchetan.com*
