# AWS Solutions Architect Associate — Concept Notes

Organized by topic. Each entry answers: **what it is**, **when to use it**, and **how to tell it apart from lookalikes**.

---

## 1. Data Transfer & Migration to AWS

| Need | Use | Why |
|---|---|---|
| Automate/accelerate **online** transfer to S3, EFS, or FSx for Windows | **AWS DataSync** | Only service natively integrated with all three; handles scheduling, retry, integrity checks, CloudWatch monitoring |
| Bulk **offline** transfer (TBs–PBs), bandwidth-constrained or disconnected site | **Snowball Edge (Storage Optimized)** | Physical device shipped to you; not for online/automated transfer |
| Fast upload/download to S3 for **large objects (>1GB)**, geographically distributed users | **S3 Transfer Acceleration (S3TA)** | Routes through CloudFront edge locations onto AWS backbone; no caching, always hits S3 |
| Fast **distribution/download** of cacheable content, **small objects (<1GB)** | **CloudFront** | CDN with caching; best for repeated downloads, not large uploads |
| SMB/NFS access to S3 for on-prem apps (gateway/hybrid, not for parallel multi-app access) | **File Gateway (Storage Gateway)** | Only supports S3, not EFS/FSx |
| File transfer directly into S3/EFS via SFTP/FTPS/FTP | **AWS Transfer Family** | Does NOT support FSx for Windows |

**Key distinction:** S3TA = large objects/no caching. CloudFront = caching, best for repeat downloads of static/smaller content. Global Accelerator = non-HTTP protocols (see networking section).

---

## 2. Directory Services (Active Directory on AWS)

| Requirement | Use |
|---|---|
| On-prem users log into AWS apps with AD creds only (no AWS-side directory-aware workloads) | **AD Connector** (just a proxy/redirector) |
| Need directory-aware workloads (e.g., SQL Server) on AWS **and** trust relationship/SSO with on-prem AD | **AWS Managed Microsoft AD** |
| ≤5,000 users, no trust relationships needed, cheapest option | **Simple AD** (Samba-based, no trust support) |
| Scalable store for hierarchical **application** data (not user directory) | **Amazon Cloud Directory** |

**Rule of thumb:** Trust relationship + workloads on AWS → Managed Microsoft AD. Just login redirection → AD Connector. Cheap/simple, no trust → Simple AD.

---

## 3. Storage Services

### S3 Transfer Acceleration vs CloudFront (uploads/downloads)
- **S3TA**: uploads AND downloads, large objects, no caching, always fresh from S3.
- **CloudFront**: downloads/distribution of cacheable (often static) content; not built for uploads.

### File/Shared Storage Services
| Need | Use |
|---|---|
| Linux-based shared file storage, NFS, EC2 (Linux AMIs only) | **Amazon EFS** |
| Windows-based shared storage, SMB, NTFS permissions, AD integration — Linux clients can also connect via SMB | **FSx for Windows File Server** |
| High-performance computing, Linux, POSIX, tight S3 integration | **FSx for Lustre** |
| Hybrid: on-prem apps need block storage backed by S3, primary data on-prem + async backup to S3 | **Storage Gateway – Volume Gateway (Stored Volume)** |
| Hybrid: on-prem apps need block storage, primary data in S3 + only hot data cached locally | **Storage Gateway – Volume Gateway (Cached Volume)** |
| Hybrid: on-prem apps need SMB/NFS access to S3 as file storage (not full block volume) | **Storage Gateway – File Gateway** |

**Cached Volume vs Stored Volume:**
- Cached = primary data in S3, hot/recent data cached locally (low-latency access to frequent data, full backup in cloud).
- Stored = primary data on-prem (full copy locally), async backup to S3 (S3 is DR copy only).

### EBS Volume Types (by IOPS/throughput need)
| Type | Max IOPS/vol | Best for |
|---|---|---|
| io1/io2 (Provisioned IOPS SSD) | up to 64,000 | Critical, high & consistent IOPS DB workloads |
| gp2/gp3 (General Purpose SSD) | up to 16,000 | Broad transactional workloads, dev/test |
| st1 (Throughput Optimized HDD) | up to 500 | Big sequential throughput — Kafka, ETL, log processing |
| sc1 (Cold HDD) | up to 250 | Rarely accessed, large cold data |

**High/specific IOPS number mentioned (e.g. 20,000+) → io1.**

### S3 Encryption
- **SSE-S3**: AWS-managed keys, automatic unique key per object (envelope encryption), zero config — best for "per-file key, no overhead" requirement.
- **SSE-KMS**: keys managed via KMS, audit trail via CloudTrail, key policies — but same key can be reused across objects.
- **SSE-C**: you supply your own key per request; AWS doesn't store it.
- **Encryption context** (used with SSE-KMS) ≠ generates new keys; it's just contextual metadata for extra integrity checking.
- **Rule:** you can copy unencrypted → encrypted, but NEVER encrypted → unencrypted (security safeguard, one-way only).

### VPC Access to S3 (cost optimization)
- **VPC Gateway Endpoint** for S3/DynamoDB: private route, no NAT gateway processing charges, no data transfer cost (same-region). Free.
- NAT Gateway: costs per GB processed — avoid routing S3-bound traffic through it when a gateway endpoint is available.
- Gateway Load Balancer endpoint: NOT usable for S3 (only for 3rd-party virtual appliances/firewalls).

### VPC Endpoint Types
- **Gateway endpoint**: S3 and DynamoDB only.
- **Interface endpoint (PrivateLink)**: most other AWS services (e.g., SQS, SNS, etc.) — private connectivity without internet.

---

## 4. Compute & Auto Recovery

### EC2 Automatic Recovery (via CloudWatch Alarm)
- Triggered by **system status check failures** (hardware/host issues), not instance status check failures.
- Recovered instance = same instance ID, private IP, Elastic IP, metadata (identical identity).
- **In-memory (RAM) data is lost** — recovery involves a reboot.
- Only works with **EBS-backed** instances — NOT instance store volumes.
- Terminated instances CANNOT be recovered.
- Up to 3 recovery attempts/day.

### Spot Instances
- **Persistent request**: reopens automatically after interruption; if you *stop* the instance, it only reopens when you manually *start* it again.
- **Spot Fleet**: by default maintains target capacity by launching replacements after terminations.
- **Cancelling a spot request** does NOT terminate the associated instance — must terminate manually (and in the right order: cancel request first, then terminate instance, for persistent requests).

### Auto Scaling Policies (esp. with SQS backlogs)
| Policy | Best for |
|---|---|
| **Target tracking** | Sudden/unpredictable spikes — continuously adjusts capacity to hit an exact target metric (e.g., SQS backlog-per-instance) |
| **Step scaling** | Approximate response to threshold breaches, reacts without cooldown wait, but less precise than target tracking |
| **Simple scaling** | Basic; must wait out a cooldown period before reacting again — poor for sudden spikes |
| **Scheduled scaling** | Predictable, recurring traffic patterns (e.g., every Wednesday) — NOT for sudden spikes |

Target tracking with SQS: use **backlog-per-instance** = ApproximateNumberOfMessages ÷ in-service instance count, targeted to an acceptable value.

### Launch Template + VPC Tenancy
- If EITHER the Launch Template tenancy OR VPC tenancy is set to **dedicated**, the resulting instance is **dedicated** (an OR rule — dedicated always wins, regardless of source).

---

## 5. Networking

### NAT Instance vs NAT Gateway
| Feature | NAT Instance | NAT Gateway |
|---|---|---|
| Type | Regular EC2 instance | Fully managed AWS service |
| Security groups | Yes (it's an EC2 instance) | No |
| Port forwarding | Yes | No |
| Can double as bastion host | Yes | No (no OS access) |
| Scaling/HA | Manual | Automatic |
| Must live in | Public subnet | Public subnet |
| AWS recommendation | Legacy | Preferred/modern choice |

**Both** let private-subnet instances initiate outbound IPv4 traffic while blocking unsolicited inbound.

### Internet Gateway
- Two jobs: (1) target in route table for internet-bound traffic, (2) performs **NAT** for instances with a public IPv4 address (translates public ↔ private IP for those instances).
- If an instance is in a **public subnet with a public IP**, the **Internet Gateway** (not a NAT device) does the address translation for it.
- NAT Gateway/instance only relevant for **private subnet** instances with no public IP.

### Egress-Only Internet Gateway
- IPv6 equivalent of a NAT gateway. Outbound only, blocks unsolicited inbound IPv6. NOT for IPv4 use cases.

### Bastion Host (Jump Box)
- Publicly accessible hardened server in the public subnet, used to SSH/RDP into private-subnet instances that have no direct internet access.
- Flow: Laptop → (internet) → Bastion (public subnet) → Private instance.
- Modern alternative: **AWS Systems Manager Session Manager** (no inbound ports needed, no bastion to manage).

### Route 53 Resolver (Hybrid DNS)
- **Inbound endpoint**: lets on-premises DNS resolvers send queries INTO AWS (on-prem → AWS VPC resolution).
- **Outbound endpoint**: lets Route 53 Resolver send queries OUT to on-prem resolvers via conditional forwarding rules (AWS VPC → on-prem resolution).
- Both AWS-side config (endpoints/rules) AND on-prem-side config (forwarding rules pointing to inbound endpoint IPs) are needed for the full hybrid setup.

### DNS Records
| Need | Use |
|---|---|
| Point subdomain to a third-party domain you don't control (not AWS resource) | **CNAME** — works only for subdomains, NOT the zone apex; Route 53 charges for CNAME queries |
| Point zone apex (root domain, e.g. `example.com`) to another record/AWS resource | **Alias record** — Route 53-specific, works at zone apex, FREE for AWS resources/same-hosted-zone targets |
| Point domain/subdomain to an IP address | **A record** |
| Resolve IP → domain (reverse DNS) | **PTR record** |

**Zone apex** = the bare/root domain (e.g. `example.com`, no `www`). DNS protocol forbids CNAME at the apex (conflicts with mandatory SOA/NS records there). Alias record was built by AWS specifically to work around this.

### AWS Global Accelerator vs CloudFront
| | Global Accelerator | CloudFront |
|---|---|---|
| Protocol | TCP **and UDP** | HTTP/HTTPS (and RTMP) |
| Use case | Gaming (UDP), VoIP, IoT/MQTT, non-HTTP, or HTTP needing static IPs/fast failover | Caching and accelerating web content (cacheable + dynamic) |
| Key feature | 2 fixed global static anycast IPs — simplifies firewall rules across many regions/ALBs | Edge caching (POPs + regional edge caches) |
| Multi-region ALB consolidation | Yes — reduces "too many IPs to whitelist" problem | Not its purpose |

Traffic flow for Global Accelerator: **User → Global Accelerator (2 static IPs) → Regional ALBs (registered as endpoints) → EC2 instances.** (Not the reverse.)

### Elastic Load Balancing — Cross-Zone Load Balancing
- **Enabled**: each LB node routes to ALL targets across ALL AZs — traffic divided evenly across total instance count.
- **Disabled**: each LB node only routes within its own AZ — traffic first split 50/50 (or evenly) across AZs, THEN divided among instances in that AZ. An AZ with fewer instances → each instance there gets MORE traffic (imbalance risk).

### Target Routing (NLB with instance ID targets)
- Traffic is routed using the **primary private IP address** on the primary network interface — NOT instance ID, NOT public IP, NOT Elastic IP. Instance ID is just a label, not a real network address.

### AZ Names vs AZ IDs
- AZ **names** (e.g., `us-west-2a`) are **randomly mapped per AWS account** — same name ≠ same physical location across accounts.
- AZ **IDs** (e.g., `usw2-az2`) are **consistent/fixed across all accounts** — use these to coordinate/match physical AZ location across multiple AWS accounts.

### Direct Connect + Transit Gateway (multi-VPC hybrid at scale)
- Many VPCs + on-prem + need full-mesh connectivity + least overhead:
  - **AWS Transit Gateway**: attach all VPCs, enable route propagation — hub-and-spoke, avoids per-VPC peering explosion.
  - **Transit VIF** (on the Direct Connect connection): connects DX directly to the Transit Gateway, replacing the need for per-VPC private VIFs.
- **DX Gateway + per-VPC VGW**: doesn't support transitive VPC-to-VPC routing; still requires managing many VGWs — more overhead.
- **PrivateLink**: exposes a single service privately, not general network-level connectivity.
- **Many individual Site-to-Site VPNs**: high overhead, lower performance, ignores existing DX capacity.

### VPN Site-to-Site Components
- **Virtual Private Gateway (VGW)**: AWS side of the VPN — attaches to your VPC.
- **Customer Gateway**: on-premises side — an AWS resource representing your on-prem device's info (not the physical device itself).

---

## 6. Security & Identity (IAM, SCP, GuardDuty, Macie, WAF, Shield)

### EC2 Access to AWS Services
- Best practice: **IAM service role + instance profile** attached to EC2 — NOT IAM user credentials stored on the instance (security anti-pattern: static keys don't expire/rotate).
- Role credentials delivered via instance metadata service — **short-lived, auto-rotated** (refreshed automatically before expiry).
- Creating an EC2 service role **auto-configures** the trust policy (EC2 is trusted automatically) — no need to manually edit the trust relationship document.

### Service-Linked Roles (SLR)
- Predefined by AWS, tied to a specific service (e.g., Auto Scaling), permissions not editable by you.
- Exist so services can perform necessary internal actions safely without you crafting custom IAM policies.

### Service Control Policies (SCP) — AWS Organizations
- Sets the **maximum permission boundary** for accounts in an OU — a guardrail, not a grant.
- Effective permission = IAM policy **AND** SCP (intersection) — both must allow.
- **Affects ALL users/roles in member accounts, INCLUDING root user of member accounts.**
- **Does NOT affect the Organization's management/master account.**
- **Does NOT affect service-linked roles** (deliberate exemption — prevents SCPs from accidentally breaking core AWS service functionality like Auto Scaling's ability to launch instances).
- Risk: an overly broad SCP CAN accidentally break application functionality for regular IAM roles — test in non-prod OU first, use IAM Access Analyzer, monitor CloudTrail for AccessDenied spikes after changes.

### Security Group valid rule sources/destinations
Valid: **IP address, CIDR range, another security group, prefix list.**
Invalid: Internet Gateway ID, Subnet ID, Route Table ID (not supported types).

### GuardDuty vs Macie vs Inspector vs Trusted Advisor vs WAF vs Shield
| Service | Purpose |
|---|---|
| **GuardDuty** | Threat detection — analyzes CloudTrail, VPC Flow Logs, DNS logs for malicious/anomalous activity (not DDoS-specific, not real-time blocking) |
| **Macie** | Sensitive data discovery/classification in S3 (PII, etc.) via ML/pattern matching |
| **Inspector** | Vulnerability scanning for EC2/containers (unpatched software, misconfig) — not network attack detection |
| **Trusted Advisor** | General recommendations (cost, security, performance) — not EC2 health checks or DDoS |
| **WAF** | Layer 7 filtering — inspects HTTP/HTTPS request content (IP, headers, query strings, patterns like SQLi/XSS); attaches to CloudFront, ALB, API Gateway, AppSync, Cognito (NOT S3 bucket policy, NOT NACL/SG-style attach to CloudFront directly as a networking construct) |
| **Shield Standard** | Free, automatic, L3/4 DDoS protection for all AWS customers |
| **Shield Advanced** | Paid — adds L7 DDoS protection, real-time visibility, automated mitigation, access to DDoS Response Team (DRT), detailed logging/reporting. Minimal architecture change (protects existing ALB/CloudFront/Route 53 in place) |

**DDoS-specific requirement + minimal architecture change + need audit/expert support → Shield Advanced** (not GuardDuty, not Inspector, not a CloudFront+WAF redesign).

### CloudFront + S3 Origin Access Control
- Use **OAI (Origin Access Identity)** or **OAC (Origin Access Control — AWS's newer recommended option)** + S3 bucket policy to force all access through CloudFront only (block direct S3 access).
- To replicate IP-based restriction (like an EC2 security group) after migrating to S3+CloudFront: need BOTH **OAI/OAC** (locks S3 to CloudFront-only) AND **WAF IP match condition on the CloudFront distribution** (enforces the actual IP filtering). Neither alone is sufficient.
- NACL and Security Group CANNOT be attached to CloudFront (CloudFront is a global edge network, not VPC-bound).

### Root User
- One root user per AWS account (tied to the account's sign-up email/credentials).
- An AWS Organization with N member accounts = N separate root users, each independently subject to SCPs (except the management account's root, which is SCP-exempt).

---

## 7. Databases (RDS, DynamoDB, Aurora)

### RDS Multi-AZ vs Read Replica
| | Multi-AZ (standby) | Read Replica |
|---|---|---|
| Replication | **Synchronous** | **Asynchronous** (always, in RDS — by AWS design) |
| Purpose | High availability / failover | Scaling READ traffic |
| Serves traffic normally? | No — standby is idle, pure backup | Yes — actively serves read queries |
| Use when | Minimize data loss, "at least 2 nodes with every transaction," reliable DB after outage | Offload read-heavy load from primary; okay with slight staleness (ms–seconds lag) |

**"Minimizes data loss" + "transaction on ≥2 nodes" → Multi-AZ, not read replica** (read replica option text claiming "synchronous" is a trap — RDS read replicas are always async).

### Reducing Replication Lag with Minimal Effort
- **Migrate RDS MySQL → Aurora MySQL**, swap read replicas for **Aurora Replicas**, enable Aurora Auto Scaling.
- Why: Aurora Replicas share the SAME underlying distributed storage volume as the primary — no separate copy to sync, so lag drops to **milliseconds** instead of seconds. MySQL-compatible = minimal app code change, still fully managed.
- Avoid: self-hosting on EC2 (ops overhead), adding a cache layer (app code changes), migrating to DynamoDB (requires query rewrite).

### DynamoDB Data Recovery
| Need | Use |
|---|---|
| Recover from unpredictable/accidental corrupted writes, restore to exact prior second | **Point-in-Time Recovery (PITR)** — continuous, automatic, 35-day window, per-second granularity |
| Manual snapshot before a known risky change | **On-demand backup** — must be triggered manually, doesn't help for unpredictable corruption |
| React to changes in near real-time (build apps that respond to item changes) | **DynamoDB Streams** — a change log (24hr retention), NOT a restore mechanism |
| Multi-region active-active — NOT a "clean region" fallback (data replicates everywhere ~1s) | **Global Tables** — one logical table, not separate independent copies |

---

## 8. Messaging & Streaming (SQS, SNS, Kinesis, EventBridge)

### Choosing the right messaging/streaming service
| Need | Use |
|---|---|
| Custom real-time stream processing/analytics apps, multiple independent consumers reading same stream | **Kinesis Data Streams** |
| Fully managed, simple "capture and load" into S3/Redshift/OpenSearch/Splunk — NO custom processing | **Kinesis Data Firehose** |
| Decouple fast producers from slow consumers; buffer/persist messages until processed; polling model | **SQS** |
| Push-based pub/sub, immediate delivery, fan-out to multiple subscribers | **SNS** |
| Event-driven integration, often with external SaaS/non-AWS services | **EventBridge** |
| No retry mechanism needed, real-time analytics workflow at scale | **Kinesis Data Streams** (has built-in retry/durability, unlike raw ingestion without one) |

**"Fast + slow process decoupling" → SQS** (buffering + independent-pace polling is the differentiator vs SNS's push model and Kinesis's pub-sub streaming model).

### SQS Cost Optimization
- **Long polling**: waits for a message (up to 20s) instead of returning immediately when empty — drastically reduces "empty receive" billable requests vs. short polling (the default).
- Not for retrieval: **visibility timeout** (hides message from other consumers after pickup) and **message timer** (delays initial visibility) — both are distractors for "how to retrieve messages."

### SQS Standard → FIFO Migration
- **Cannot convert in-place** — must delete and recreate, or create new FIFO queue.
- FIFO queue name **must end in `.fifo`** (mandatory suffix) — so name cannot match the original standard queue name exactly.
- Throughput: **300 msgs/sec without batching**, **3,000 msgs/sec with batching**.
- Exceeding the without-batching limit → requests get **throttled** (explicit error, e.g. ThrottlingException) — not silently dropped; app should retry with backoff or switch to batching (SendMessageBatch, up to 10 messages/call).

---

## 9. Caching (ElastiCache)

**Good fit for:**
- Read-heavy workloads (leaderboards, social feeds, gaming, Q&A portals) — store frequently-read objects in-memory.
- Compute-intensive workloads (e.g., recommendation engines, complex scoring algorithms) — cache the expensive-to-compute result instead of recalculating every request.

**NOT a fit for:**
- Write-heavy workloads (cache goes stale too fast to be useful).
- ETL workloads (use AWS Glue or EMR instead).
- Complex JOIN queries (use RDS/Aurora — relational engines).

---

## 10. Monitoring & Config (CloudWatch vs CloudTrail vs Config vs Systems Manager)

| Service | Answers the question... |
|---|---|
| **CloudWatch** | "How is my resource performing right now? Any alerts?" — metrics, alarms, performance monitoring |
| **CloudTrail** | "Who did what API action, and when?" — account activity/audit log |
| **AWS Config** | "What did this resource look like at time X? Is it compliant?" — configuration history + compliance evaluation |
| **Systems Manager** | Operational tasks — grouping resources, running commands, patch management (not config history) |

### Systems Manager — Patch Management on Existing Fleet
- If EC2 instances already have an IAM role doing other things (e.g., RDS/Secrets Manager access) and you want to ADD SSM management (patching) without touching that role or causing disruption:
  - Use **Default Host Management Configuration (Systems Manager Quick Setup)** — auto-configures required SSM permissions/inventory/patching SEPARATELY from the existing role, no manual IAM editing, no risk to existing app functionality.
  - Avoid: manually replacing/merging the IAM role (risk of downtime), attaching two IAM roles to one instance (not possible — only one role per instance), manual SSM Agent install + cron (reinvents the wheel, no centralized compliance reporting).
- Hybrid Activations = for non-EC2/on-premises servers only, not standard EC2 management.

---

## 11. Compute — Containers & Serverless Storage

### Fully Managed Containerized App Needing Persistent/Shared Storage
- **ECS with Fargate** (no EC2/servers to manage) + **EFS** (mountable, persistent, shared file system) — the standard "fully managed container + persistent storage" combo.
- Avoid: EKS with managed node groups (still requires managing underlying EC2 instances), S3 "mounted" into a container (S3 is object storage, not a real file system — can't be mounted natively), Lambda + /tmp (ephemeral, 512MB cap, wiped between invocations — not for stateful/long-running apps).

---

## 12. AMI (Amazon Machine Image)

- **Can copy across AWS Regions** (via CopyImage action — console/CLI/SDK/API).
- **Can share an AMI with another AWS account** (via launch permissions).
- **Encryption during copy is one-way**: unencrypted → encrypted is allowed; encrypted → unencrypted is NEVER allowed (security safeguard against accidentally stripping protection).

---

## Quick Cross-Reference: "Sounds Similar But Isn't"

- **CNAME vs Alias record** — CNAME = standard DNS, works for subdomains only, costs money in Route 53. Alias = Route53-specific, works at zone apex too, free for AWS targets.
- **NAT Gateway vs Internet Gateway** — NAT Gateway = private subnet instances reaching internet (no public IP). Internet Gateway = does NAT for public-subnet instances that already have a public IP, plus routes traffic.
- **NAT Instance vs NAT Gateway** — Instance = real EC2 (SG, port forwarding, bastion-capable). Gateway = managed service (none of that).
- **VPC Gateway Endpoint vs Interface Endpoint (PrivateLink)** — Gateway = S3/DynamoDB only, free. Interface = most other services, uses ENI/PrivateLink.
- **GuardDuty vs Macie** — GuardDuty = threat/malicious activity detection. Macie = sensitive data discovery/classification.
- **WAF vs Shield** — WAF = L7 content-based filtering (rules on request characteristics). Shield = DDoS-specific volumetric attack mitigation (L3/4, and L7 with Advanced), includes DRT with Advanced tier.
- **Kinesis Data Streams vs Firehose** — Streams = build custom consumer apps, real-time processing. Firehose = fully managed, just loads into a destination, no custom processing.
- **SQS vs SNS vs EventBridge** — SQS = polling/buffering, decouple mismatched speeds. SNS = push pub/sub, immediate fan-out. EventBridge = event-driven integration, especially with SaaS/external sources.
- **AZ name vs AZ ID** — Name is per-account random mapping. ID is globally consistent per physical location.
- **Global Accelerator vs CloudFront** — Accelerator = TCP/UDP, non-HTTP, static IPs, multi-region ALB consolidation. CloudFront = HTTP-based caching/CDN.
