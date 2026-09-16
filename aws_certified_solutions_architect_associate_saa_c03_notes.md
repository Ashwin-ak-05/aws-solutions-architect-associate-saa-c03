# AWS Certified Solutions Architect - Associate (SAA-C03)
**Course Notes by Chetan Agrawal**

---

## 1. Getting Started with AWS

### AWS Global Infrastructure
To deploy and run any application, we need Compute, Storage, and Network.
*   **Regions:** Geographic locations around the world where AWS clusters data centers. Chosen based on:
    *   Low latency for end users
    *   Data Residency & Compliance
    *   Disaster Recovery
    *   AWS services pricing difference
    *   Availability of AWS services
    *   *Naming:* e.g., N. Virginia = `us-east-1`, Mumbai = `ap-south-1`.
*   **Availability Zones (AZs):** 
    *   1 Region = Multiple Availability Zones (minimum 3).
    *   1 AZ = Multiple data centers (redundant power, networking, housed in separate floodplains).
    *   Purpose: High Availability, Failover, Durability, Scaling, and load distribution.
*   **Edge Locations:** Used by CloudFront (CDN) to cache data closer to end-users.
*   **Local Zones, Wavelength Zones (5G), Outposts (on-prem).**

### AWS Core Services
*   **Compute:** EC2
*   **Storage:** S3
*   **Networking:** VPC
*   **Security:** IAM

---

## 2. AWS Identity and Access Management (IAM)

*   **Global Service:** IAM is a global service and is free to use.
*   **Root User:** Owner of the AWS account. Unrestricted access. Not recommended for day-to-day operations. Used for account closure, billing access, changing support plans, etc.
*   **IAM User:** Individual users or applications with specific permissions.
*   **IAM Group:** A collection of IAM users. Permissions attached to the group apply to all users in it.
*   **IAM Role:** An identity that can be assumed temporarily by users, applications, or services (e.g., EC2, Lambda) to access AWS resources without long-term credentials.

### Accessing AWS
1.  **AWS Management Console:** (Username + Password + MFA)
2.  **AWS CLI (Command Line Interface):** (Access Key ID + Secret Access Key)
3.  **AWS SDK (Software Development Kit):** (Access Key ID + Secret Access Key)

### IAM Policies
*   JSON documents that define permissions.
*   **Identity-based policy:** Attached to IAM user, group, or role.
*   **Resource-based policy:** Attached to AWS resources (e.g., S3 bucket policy). Includes a `Principal` element.
*   **Implicit Deny:** By default, all access is denied.
*   **Explicit Deny:** An explicit deny always overrides an allow.

**Sample IAM Policy Structure:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["ec2:StartInstances", "ec2:StopInstances"],
      "Resource": "arn:aws:ec2:region:account-id:instance/*",
      "Condition": {
        "StringEquals": { "aws:RequestedRegion": "ap-south-1" }
      }
    }
  ]
}
```

### Advanced IAM Concepts
*   **Permissions Boundary:** Sets the *maximum* permissions an IAM user or role can have. It does not grant permissions by itself.
*   **IAM Access Analyzer:** Monitors policies and detects unused access, public access, or cross-account access.
*   **IAM Policy Simulator:** Tests IAM policies before applying them to troubleshoot AccessDenied issues.

---

## 3. Amazon EC2 (Elastic Compute Cloud)

### EC2 Configuration Options
*   **AMI (Amazon Machine Image):** OS template (Linux, Windows).
*   **Instance Type:** CPU, Memory capabilities (e.g., `t2.micro`, `c6i.xlarge`).
*   **Storage (EBS / Instance Store):**
    *   **EBS (Elastic Block Store):** Network drive, persists data after EC2 termination.
    *   **Instance Store:** Physical drive attached to the host. Data is wiped if EC2 is stopped/terminated.
*   **Network:** VPC, Subnet, Public/Private IP, Elastic IP.
*   **Security Groups:** Firewall attached to the EC2 instance.
*   **SSH Key Pair:** Used to securely log into the instance (Private/Public key).
*   **User Data:** Bootstrap script run automatically at the time of first launch (runs as root).

### EC2 Pricing Models
1.  **On-Demand:** Pay by the second/hour. No commitment. Best for unpredictable, spiky workloads.
2.  **Spot Instances:** Up to 90% discount. Spare capacity. Can be interrupted with a 2-minute warning. Best for stateless, batch jobs, CI/CD.
3.  **Reserved Instances (RI):** 1 or 3-year commitment. Up to 72% discount. Best for steady-state databases/apps.
4.  **Savings Plans:** 1 or 3-year commitment to a specific dollar amount per hour ($/hr). Applies across EC2, Fargate, Lambda.
5.  **Dedicated Hosts/Instances:** Hardware dedicated to your account. Used for strict compliance or BYOL (Bring Your Own License).

### Security Groups vs Network ACLs
| Feature | Security Group (SG) | Network ACL (NACL) |
| :--- | :--- | :--- |
| **Operates at** | EC2 Instance level | Subnet level |
| **Rules** | Supports ONLY Allow rules | Supports Allow AND Deny rules |
| **State** | **Stateful:** Return traffic is automatically allowed | **Stateless:** Return traffic must be explicitly allowed |
| **Evaluation** | All rules evaluated before a decision | Rules evaluated in order (lowest number first) |

### Elastic Block Store (EBS) Advanced
*   **Snapshots:** Point-in-time backup of EBS volumes stored in S3.
*   **Encryption:** Handled by AWS KMS. Encrypts data at rest, data in transit, and snapshots.
*   **Multi-Attach:** Allows a single `io1` or `io2` volume to be shared by up to 16 EC2 instances in the same AZ.

---

## 4. Elastic Load Balancing (ELB) & Auto Scaling

### Load Balancer Types
1.  **Application Load Balancer (ALB) - Layer 7:** HTTP, HTTPS, WebSocket, gRPC. Content-based routing (Host, Path, Query String).
2.  **Network Load Balancer (NLB) - Layer 4:** TCP, UDP, TLS. Ultra-low latency, provides a Static IP per AZ.
3.  **Gateway Load Balancer (GWLB) - Layer 3:** GENEVE protocol. Used to deploy and scale 3rd-party network virtual appliances (Firewalls).
4.  **Classic Load Balancer (CLB):** Legacy.

### Advanced ELB Features
*   **Cross-Zone Load Balancing:** Distributes traffic evenly across all registered targets in all enabled AZs. (Default ON for ALB, OFF for NLB).
*   **SNI (Server Name Indication):** Allows hosting multiple SSL certificates on a single load balancer.
*   **X-Forwarded-For:** Header used by ALB to pass the original Client IP to the backend EC2 instance.

### Auto Scaling Groups (ASG)
*   Automates the scaling of EC2 instances (Min, Max, Desired capacity).
*   **Scaling Policies:**
    *   *Target Tracking:* Maintains a metric at a specific value (e.g., Avg CPU at 50%).
    *   *Step Scaling:* Scales in discrete steps based on CloudWatch alarms.
    *   *Scheduled Scaling:* Scales based on a predefined schedule.
    *   *Predictive Scaling:* Uses ML to forecast and scale in advance.

---

## 5. VPC and Networking

*   **VPC (Virtual Private Cloud):** Logically isolated virtual network.
*   **Subnets:** Partition of the VPC network inside a specific AZ.
    *   *Public Subnet:* Has a route to the Internet Gateway (IGW).
    *   *Private Subnet:* Does not have a direct route to the IGW.
*   **Internet Gateway (IGW):** Connects VPC to the internet.
*   **NAT Gateway:** Allows instances in a Private Subnet to access the internet (for updates, etc.) but blocks incoming internet connections. Highly available within a single AZ. Must be placed in a *Public* Subnet.

### VPC Private Connectivity
*   **VPC Peering:** Connects two VPCs. Not transitive. CIDR blocks cannot overlap.
*   **VPC Endpoints (AWS PrivateLink):** Connect to AWS services privately without an IGW or NAT Gateway.
    *   *Gateway Endpoint:* Used ONLY for **S3** and **DynamoDB**. Requires route table updates. Free.
    *   *Interface Endpoint:* Used for all other services (SQS, SNS, Kinesis, etc.). Creates an ENI in the subnet. Paid.
*   **Transit Gateway:** Centralized hub-and-spoke router connecting thousands of VPCs and on-prem networks. Supports IP Multicast.
*   **Site-to-Site VPN:** Encrypted IPSec connection over the public internet between on-prem and AWS.
*   **Direct Connect (DX):** Dedicated, private physical network connection between on-prem and AWS. Takes weeks/months to setup.

---

## 6. Amazon S3 (Simple Storage Service)

*   **Object Storage:** Flat structure, virtually unlimited storage, 99.999999999% (11 9's) durability.
*   **Storage Classes:**
    *   *S3 Standard:* Frequent access, millisecond latency.
    *   *S3 Standard-IA:* Infrequent access, lower storage cost but retrieval fee. Minimum 30 days.
    *   *S3 One Zone-IA:* Recreated data, stored in a single AZ.
    *   *S3 Intelligent-Tiering:* Auto-moves data between access tiers based on usage. No retrieval fees.
    *   *S3 Glacier (Instant, Flexible, Deep Archive):* For archiving. Deep archive takes up to 12 hours for retrieval.
    *   *S3 Express One Zone:* Single-digit millisecond latency for high-performance apps (e.g., ML training).

### S3 Security & Features
*   **Bucket Policies:** JSON policies attached to buckets to grant/deny cross-account or public access.
*   **Versioning:** Protects against accidental overwrites/deletions. Required for Replication and Object Lock.
*   **Object Lock:** WORM (Write Once Read Many) model. Prevents deletion for compliance (Governance vs. Compliance mode).
*   **Encryption:** 
    *   *SSE-S3:* AWS managed keys (Default).
    *   *SSE-KMS:* Uses AWS KMS keys.
    *   *SSE-C:* Customer provided keys.
*   **Lifecycle Rules:** Automates moving objects to colder storage tiers or expiring/deleting them.
*   **Replication:** SRR (Same Region) and CRR (Cross Region). Versioning must be enabled.
*   **Event Notifications:** Triggers SNS, SQS, or Lambda upon object creation/deletion.
*   **Pre-signed URLs:** Grants temporary access to download/upload an object without requiring AWS credentials.

---

## 7. Databases

### Relational Databases (OLTP)
*   **Amazon RDS:** Managed DB (MySQL, PostgreSQL, Oracle, SQL Server, MariaDB).
    *   *Multi-AZ:* For **High Availability** (synchronous standby).
    *   *Read Replicas:* For **Read Scaling** (asynchronous). Up to 15 replicas.
*   **Amazon Aurora:** AWS native, 5x faster than MySQL, 3x faster than PostgreSQL. Decoupled compute and storage.
    *   *Aurora Global Database:* Cross-region replication for low latency global reads & DR.
    *   *Aurora Serverless:* Auto-scales capacity based on load. Perfect for unpredictable workloads.

### NoSQL Databases
*   **Amazon DynamoDB:** Key-value/document DB. Single-digit millisecond latency at any scale. Serverless.
    *   *Global Tables:* Active-Active multi-region replication.
    *   *DAX (DynamoDB Accelerator):* In-memory cache for microsecond latency.
    *   *TTL (Time To Live):* Auto-expires old items.
*   **Amazon DocumentDB:** MongoDB compatible document database.

### Other Databases
*   **Amazon ElastiCache:** In-memory caching (Redis / Memcached) to offload database read traffic.
*   **Amazon Redshift:** Petabyte-scale Data Warehouse (OLAP). Columnar storage.
*   **Amazon Neptune:** Graph database (Social networks, recommendation engines, fraud detection).
*   **Amazon QLDB:** Quantum Ledger Database (Immutable, cryptographically verifiable transaction logs).
*   **Amazon Timestream:** Time-series database (IoT, operational metrics).

---

## 8. AWS Serverless & Integration

### Serverless Compute
*   **AWS Lambda:** Run code without provisioning servers. Pay per invocation & duration.
    *   *Concurrency:* Reserved Concurrency (prevents throttling), Provisioned Concurrency (prevents cold starts).
    *   *SnapStart:* 10x faster startup for Java functions.
*   **Amazon API Gateway:** Create, publish, maintain REST, HTTP, and WebSocket APIs.
    *   *Integration:* Lambda, HTTP, AWS Services, Mock, VPC Link.
    *   *Security:* IAM, Cognito User Pools, Lambda Custom Authorizers.

### Application Integration (Loosely Coupled)
*   **Amazon SQS (Simple Queue Service):** Message queue for decoupling applications.
    *   *Standard:* At-least-once delivery, best-effort ordering.
    *   *FIFO:* Exactly-once processing, strict ordering.
    *   *Visibility Timeout:* Time a message is hidden from other consumers while being processed.
    *   *DLQ (Dead Letter Queue):* For messages that fail processing multiple times.
*   **Amazon SNS (Simple Notification Service):** Pub/Sub model. Fan-out architecture (One message triggers SQS, Lambda, Emails).
*   **Amazon EventBridge:** Serverless event bus. Routes events between AWS services, SaaS, and custom apps based on rules/patterns.

---

## 9. Big Data & Analytics

*   **Amazon Athena:** Serverless interactive query service. Run SQL queries directly against data in S3.
*   **AWS Glue:** Serverless Data Integration & ETL (Extract, Transform, Load). Includes Data Catalog and Crawlers.
*   **Amazon EMR (Elastic MapReduce):** Big data platform for running Apache Spark, Hadoop, Presto, etc., on EC2/EKS.
*   **Amazon Kinesis:**
    *   *Data Streams:* Real-time streaming data ingestion (Shards, partition keys).
    *   *Data Firehose:* Delivery of streaming data to S3, Redshift, OpenSearch (can transform via Lambda).
*   **Amazon QuickSight:** Serverless BI and dashboarding tool. Uses SPICE in-memory engine.
*   **AWS Lake Formation:** Centralized data access control and permissions management for Data Lakes.

---

## 10. Edge Networking & Content Delivery

### Amazon CloudFront
*   Global Content Delivery Network (CDN) caching static/dynamic content at edge locations.
*   **Origins:** S3, ALB, EC2, API Gateway.
*   **Security:** Enforces HTTPS, integrates with WAF and Shield, Geo-Restriction.
*   **OAC (Origin Access Control):** Secures S3 origins so they can ONLY be accessed via CloudFront.
*   **Edge Compute:** 
    *   *CloudFront Functions:* Ultra-low latency, simple JS for header manipulation (Viewer Request/Response).
    *   *Lambda@Edge:* More complex Node.js/Python logic, external API calls (All 4 trigger points).

### Amazon Route 53
*   Highly available DNS service.
*   **Routing Policies:**
    *   *Simple:* One record, multiple IPs.
    *   *Failover:* Active/Passive using Health Checks.
    *   *Weighted:* Traffic splitting (e.g., 80/20).
    *   *Latency:* Routes to the region with lowest network latency.
    *   *Geolocation:* Routes based on user's country/continent.
    *   *Geoproximity:* Routes based on geographic distance with "Bias" shifting.
*   **CNAME vs Alias:** Alias is AWS-specific, maps a domain to an AWS resource (like an ALB), and is free/supports zone apex.

### AWS Global Accelerator
*   Improves global application availability and performance using the AWS global network.
*   Provides 2 Static Anycast IPs.
*   Routes TCP/UDP traffic to the optimal regional endpoint based on health and geography.

---

## 11. Security & Management

### Security Services
*   **AWS WAF:** Protects against Layer 7 attacks (SQLi, XSS, rate limiting).
*   **AWS Shield:** DDoS protection (Standard is free; Advanced is paid for Layer 3/4/7).
*   **Amazon GuardDuty:** Intelligent threat detection (ML) analyzing VPC Flow Logs, CloudTrail, DNS logs.
*   **Amazon Inspector:** Automated vulnerability management (scans EC2, ECR, Lambda for CVEs).
*   **AWS Security Hub:** Centralized security dashboard aggregating alerts.
*   **Amazon Macie:** Uses ML to discover and protect sensitive data (PII) in S3.
*   **AWS KMS:** Key Management Service (Envelope Encryption).
*   **AWS Secrets Manager:** Stores and automatically rotates database credentials.

### Management & Monitoring
*   **Amazon CloudWatch:** 
    *   *Metrics:* CPU, Network, Disk I/O.
    *   *Alarms:* Trigger Auto Scaling or SNS.
    *   *Logs:* Centralized log storage (requires CloudWatch Agent on EC2).
*   **AWS CloudTrail:** API auditing. Records "Who did what, when, and from where" in the AWS account.
*   **AWS Config:** Records configuration changes to AWS resources. Evaluates compliance against rules.
*   **AWS Systems Manager (SSM):** 
    *   *Session Manager:* Secure shell access to EC2 without SSH keys or opening port 22.
    *   *Parameter Store:* Stores configuration data (plain text or encrypted).

---

## 12. Storage & Migration

*   **Amazon EFS (Elastic File System):** Managed NFS for Linux. Multi-AZ, can be attached to hundreds of EC2 instances simultaneously.
*   **Amazon FSx:**
    *   *FSx for Windows File Server:* Native Windows SMB file system with AD integration.
    *   *FSx for Lustre:* High-performance parallel file system for HPC / Machine Learning. Linked to S3.
*   **AWS Storage Gateway:** Hybrid storage. Connects on-prem environments to AWS storage (S3 File Gateway, Volume Gateway, Tape Gateway).
*   **AWS DataSync:** Automates moving massive amounts of data between on-prem (NFS/SMB) and AWS (S3/EFS/FSx).
*   **AWS Transfer Family:** Managed SFTP, FTPS, FTP services backed by S3 or EFS.
*   **AWS Application Migration Service (MGN):** Lift-and-shift migration for servers. Continuous block-level replication.
*   **AWS Database Migration Service (DMS):** Replicates databases. Use Schema Conversion Tool (SCT) for heterogeneous migrations (e.g., Oracle to Aurora).

---

## 13. Multi-Account Management

*   **AWS Organizations:** Centrally manage multiple AWS accounts. Consolidated billing.
*   **Service Control Policies (SCPs):** Set maximum boundaries for IAM permissions across accounts/OUs. (Does NOT grant permissions).
*   **AWS Control Tower:** Automates setting up a secure, multi-account "Landing Zone" with built-in best practice guardrails.
*   **AWS IAM Identity Center (formerly SSO):** Centralized login for all AWS accounts and business apps using AD or external IdP.