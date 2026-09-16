# AWS Solutions Architect Associate — Study Notes
*Compiled from practice question review session*

---

## 1. COMPUTE — EC2

### Placement Groups
| Type | What it does | When to use |
|---|---|---|
| **Cluster** | Packs instances close together in ONE AZ, same high-bandwidth network segment (up to 10 Gbps per flow) | HPC, tightly-coupled node-to-node communication, need LOW LATENCY + HIGH THROUGHPUT between instances |
| **Spread** | Places instances on distinct racks/hardware (max 7 per AZ) | Need to MINIMIZE correlated failures / fault isolation, not performance |
| **Partition** | Groups instances into partitions; partitions don't share hardware | Large distributed systems like Hadoop, Cassandra, Kafka |

**Trigger words:** "tightly coupled," "low latency," "high throughput between instances" → **Cluster**. "Avoid correlated failures" → **Spread**.

### EFA vs ENA
- **ENA (Elastic Network Adapter):** Enhanced networking (SR-IOV), higher bandwidth/PPS, lower latency — general purpose.
- **EFA (Elastic Fabric Adapter):** Everything ENA has PLUS **OS-bypass** — apps talk directly to hardware. Built for **HPC and Machine Learning** workloads needing extreme low-latency inter-instance comms.
- **Trigger:** "HPC," "machine learning," "tightly-coupled compute" → **EFA** (superset of ENA).

### Spot vs On-Demand vs Reserved vs Dedicated
| Type | Cost | Availability | Use when |
|---|---|---|---|
| **On-Demand** | Full price | Guaranteed | Unpredictable workloads, can't be interrupted |
| **Spot** | Up to 90% cheaper | Can be reclaimed (2-min warning) | Flexible, fault-tolerant, batch/background jobs |
| **Reserved Instance (RI)** | Discounted (commit 1-3 yrs) | Guaranteed | Steady-state, predictable, long-term workload (e.g., ASG minimum capacity that's always running) |
| **Dedicated Instances** | Cheaper than Dedicated Hosts | Isolated hardware (not shared across AWS accounts) | Compliance requiring single-tenant hardware, cost matters most |
| **Dedicated Hosts** | Most expensive | Full visibility/control of physical server | Need to **bring existing server-bound software licenses** (per-socket/core licensing) |

**Key distinction:** Dedicated Instances = tenant isolation only. Dedicated Hosts = isolation + control over exact physical server (needed for BYOL).

### Instance Store vs EBS
- **Instance Store:** Physically attached to host, ephemeral (data lost on stop/terminate), **fastest**, **free** (included in instance cost), comes with specific instance types only (not addable later).
- **EBS:** Network-attached, persistent, costs extra, needed for root volumes and anything that must survive instance stop/terminate.
- **Trigger:** "Temporary/scratch storage," "high IOPS," "cost-optimal," data doesn't need to persist → **Instance Store**.
- Most instances use BOTH: EBS root volume (OS) + optional Instance Store (scratch data).

### EBS Volume Types
| Type | Best for | Notes |
|---|---|---|
| **gp2/gp3 (General Purpose SSD)** | Broad range of workloads, cost-effective | Burst credit model (gp2); baseline scales with size |
| **io1/io2 (Provisioned IOPS SSD)** | I/O-intensive DBs, need CONSISTENT/GUARANTEED IOPS | No burst — you pay for guaranteed performance. **Only type that supports Multi-Attach** |
| **st1 (Throughput Optimized HDD)** | Big data, data warehouses — sequential throughput | Not for random I/O |
| **sc1 (Cold HDD)** | Infrequently accessed data, lowest cost | Lowest performance |

**EBS Multi-Attach:** Only supported on **io1/io2**, same AZ only, **cannot be used as root volume** — each instance still needs its own separate root volume; Multi-Attach volume is an ADDITIONAL shared data volume requiring a cluster-aware filesystem/app.

**gp2 → gp3/io1 cost question pattern:** If workload is under-utilized with occasional bursts, switch **io1 → gp2** to save cost (gp2 handles bursts via credits, cheaper than paying for guaranteed IOPS you don't need).

### AMI (Amazon Machine Image)
- Contains a FULL snapshot of the disk: OS + all installed software + all files — not just config.
- Built from EBS snapshot(s) of root volume + any other attached EBS volumes (unless excluded).
- **AMIs are Region-specific** — must be manually copied to other Regions for multi-Region DR.
- **Launch Template** ≠ AMI: Launch Template is just a saved config (instance type, security group, key pair, AND a reference to which AMI to use) — contains NO actual data itself.
- **Fast RTO + multi-Region DR:** Create AMI → copy to all needed Regions → launch from Region-specific AMI when needed.

### EC2 Instance Connect
- SSH access using a **temporary, one-time public key** injected for ~60 seconds — no long-term key pairs to manage.
- **Without Session Manager:** requires the instance to have a **public IP**, connects over the internet.
- **Private IP access:** requires **Session Manager + SSM Agent** running on the instance.
- **EC2 Instance Connect Endpoint:** only needed for **private-subnet instances with NO public IP** — provides a managed entry point without exposing the instance. NOT needed if the instance already has a public IP.

### ASG (Auto Scaling Group)
- **Scale-out** = add instances (demand up). **Scale-in** = remove instances (demand down).
- **Default termination policy order (scale-in):**
  1. Balance AZs first (terminate from AZ with MORE instances)
  2. Within that AZ, terminate instance with OLDEST launch template/config
  3. Tiebreaker: closest to next billing hour
  - Never truly random.
- **Minimum capacity for HA:** Use **2** (spread across 2 AZs) — not 1 (no HA) and not 3 (wasteful, 2 is already HA-sufficient).
- **RIs for ASG minimum capacity:** Since minimum/baseline instances always run, they're prime candidates for Reserved Instance discounts.
- **ASG + ALB health check mismatch:** If ASG uses default EC2 health check (checks instance status only) while ALB uses its own (app-level) health check, ALB may remove an unhealthy instance from rotation, but ASG won't replace it (since EC2-level status still looks fine). **Fix:** configure ASG to use ELB health checks too.
- **Single-instance-only apps (monolith, can't distribute) needing AZ failure recovery, cost-optimized:**
  - ASG spanning 2 AZs with **min=1, max=1, desired=1** (exactly one instance, auto-replaced in the other AZ on failure)
  - **Elastic IP + EC2 user-data script** to reattach EIP to the new instance (cheaper than an ALB when there's only ever one instance)
  - **EC2 Instance Role** (IAM role) so the user-data script has permission to make the EIP reattachment API call
  - Do NOT use ALB here — unnecessary cost when routing to only one target ever exists.

### Logs surviving instance termination
- **Problem:** ASG terminates instances frequently; logs stored locally are lost.
- **Fix:** Install the **CloudWatch Logs agent** on instances to continuously stream logs off-instance to CloudWatch Logs — decouples logs from instance lifecycle.
- Wrong: snapshotting before termination (wasteful/expensive), Lambda SSHing in periodically (fragile/complex), disabling termination (defeats ASG elasticity).

### User Data
- Runs **once, by default, only during first boot/launch** — no extra config needed for "run once."
- To run on EVERY restart, you must explicitly reconfigure — that's the non-default behavior.
- **Instance metadata** ≠ script execution — metadata is just descriptive info about the instance (instance ID, AMI ID, IPs) queryable via a special endpoint; you CANNOT run custom scripts through metadata.

---

## 2. STORAGE — S3, EBS, EFS, Glacier

### S3 Storage Classes — When to Use What
| Class | Access pattern | Key trait |
|---|---|---|
| **S3 Standard** | Frequent | Highest cost, multi-AZ |
| **S3 Standard-IA** | Infrequent, but need INSTANT/rapid access when needed | Retrieval fee, min 30-day storage charge, still multi-AZ |
| **S3 One Zone-IA** | Infrequent, data is reproducible/non-critical | Single AZ only — NEVER use for critical/irreplaceable data or "high availability across AZ failure" requirements |
| **S3 Intelligent-Tiering** | UNPREDICTABLE/mixed access pattern | Auto-moves objects between tiers based on actual usage; small monitoring fee; no retrieval fee |
| **S3 Glacier Instant/Flexible Retrieval** | Rarely accessed, retrieval OK to wait (mins-hours) | NEVER use when "immediate access always required" is stated |
| **S3 Glacier Deep Archive** | Long-term archive, cheapest | Longest retrieval time |

**Golden rule:** "Immediate/rapid access always required" → rules out ALL Glacier tiers. "Data is critical/hard to reproduce" → rules out One Zone-IA. "Unpredictable access pattern" → Intelligent-Tiering. "Known/predictable pattern" (e.g., "rarely accessed after 30 days") → manual Lifecycle Policy to Standard-IA/Glacier is cheaper than Intelligent-Tiering's monitoring fee.

### S3 Lifecycle Policies
- Use a **prefix** to target lifecycle rules to only part of a bucket (when different folders/prefixes have different access patterns).
- No prefix needed when the SAME rule applies to the entire bucket (e.g., "delete everything after 5 years").
- If retention ends in **deletion**, don't archive to Glacier right before deleting — wasteful. Just delete directly from the last active tier.

### S3 Storage Class Analysis
- **ONLY** gives recommendations for **Standard → Standard-IA** transitions. Does NOT recommend One Zone-IA or any Glacier tier.

### S3 Storage Lens
- **Org-wide, cross-account, cross-Region visibility** into S3 usage/configuration metrics (including versioning status, storage class distribution, etc.)
- Use for: "identify all buckets across accounts/Regions missing X configuration" (e.g., versioning) — scalable, minimal manual effort.
- NOT a compliance/access tool — that's IAM Access Analyzer (different purpose: resource sharing/access).

### S3 Object Lock — Compliance vs Governance Mode
| Mode | Who can override/delete during retention |
|---|---|
| **Compliance Mode** | **NOBODY** — not even root. True WORM. Use for strict regulatory requirements. |
| **Governance Mode** | Privileged users with `s3:BypassGovernanceRetention` CAN override. Use for operational flexibility, NOT strict compliance. |

- Versioning + bucket policy denying delete = NOT tamper-proof (policy itself can be changed by someone with permissions).
- **Glacier Vault Lock** = the archival-storage equivalent of Object Lock, for Glacier vaults specifically (low-cost + long-term + locked).

### S3 Bucket Keys (cost optimization with SSE-KMS)
- Problem: SSE-KMS makes a KMS API call per object — expensive at high volume.
- **S3 Bucket Keys**: generates a bucket-level key used locally by S3 to derive per-object keys, cutting KMS request costs up to 99% — while STILL using KMS (unlike switching to SSE-S3, which drops KMS entirely).
- Use when: "reduce KMS cost but MUST keep using KMS" is the requirement.

### S3 Encryption Types
| Type | Who manages the key | Who does encryption | Custom algorithm? |
|---|---|---|---|
| **SSE-S3** | AWS | AWS | No |
| **SSE-KMS** | AWS KMS (customer-managed CMK possible) | AWS | No — standard AES |
| **SSE-C** | Customer provides key | AWS | No — AWS still encrypts, using its algorithm |
| **Client-Side Encryption** | Customer | **Customer, before upload** | **YES — only option supporting a proprietary/custom algorithm** |

**Trigger:** "Company's own proprietary encryption algorithm" → **Client-Side Encryption**, always (only option where AWS never touches the encryption process).

### EFS (Elastic File System)
- **Mount targets:** ONE per AZ (not per subnet) — pick one subnet within the AZ to host it; other subnets in the SAME AZ can still reach it with low latency (same-AZ traffic stays local).
- Cross-AZ access = higher latency + cost — always use the mount target in your OWN AZ.

### S3 vs EFS vs FSx (cost optimization / refactoring)
- If team is OPEN to refactoring and needs cost optimization for unpredictable/infrequent access → **migrate off EFS to S3 with Intelligent-Tiering** (cheaper than EFS Standard-IA).
- **FSx for Lustre** = HPC/ML high-performance compute storage — NOT for cost-optimized archival.
- **FSx for NetApp ONTAP** = enterprise NAS / lift-and-shift NetApp migrations — overkill/pricier than S3 for simple cost optimization.

### Glacier / Snowball Facts
- **Snowball Edge cannot copy directly into Glacier** — must copy into **S3 first**, then use a Lifecycle Policy to transition into Glacier.
- **Petabyte-scale one-time migration** → Snowball family (physical device), NOT Direct Connect (too slow/costly to provision for one-time) or Site-to-Site VPN (too low bandwidth).

### S3 Access Points vs Gateway VPC Endpoint
- **S3 Access Points**: simplify/manage ACCESS PERMISSIONS to shared buckets — does NOT change the network path (still goes over public internet unless paired with a VPC endpoint).
- **Gateway VPC Endpoint for S3**: provides actual PRIVATE network path from VPC to S3 (no public internet). FREE. Update route table; no app changes needed.
- **Trigger:** "Avoid using S3's public endpoint / keep traffic private" → **Gateway VPC Endpoint**, not Access Points, not NAT Gateway (NAT still uses S3's public endpoint), not Direct Connect (that's for on-prem, this is intra-AWS).

### Gateway Endpoint vs Interface Endpoint
- **Gateway Endpoint**: ONLY for **S3 and DynamoDB**. Free. Uses route table.
- **Interface Endpoint**: For almost every OTHER AWS service (SQS, SNS, Kinesis, etc.). Costs money (hourly + data). Uses an ENI with private IP in your subnet.
- Neither means the service itself "runs inside" your VPC — both just give your VPC a private path to reach an external AWS-managed service.

---

## 3. DATABASES

### RDS Multi-AZ — Precise Mechanics
- Standby replication is **SYNCHRONOUS** (not async — that's Read Replicas).
- **Standby is NEVER queryable** — pure failover target, no read/write access. (Tested repeatedly — do not confuse with Read Replica.)
- Failover: **automatic**, RDS flips the **CNAME** to point to the standby (now promoted to primary). **URL/endpoint stays the SAME** for your app — no manual reconfig needed.
- Backups ARE taken from the standby → primary's I/O is NOT interrupted during backup windows.
- OS maintenance pattern: patch standby → promote standby to primary → patch old primary (now standby). Minimizes downtime.

### RDS Read Replica
- **Asynchronous** replication (contrast with Multi-AZ's synchronous).
- **IS queryable** — used to offload READ-heavy workloads (reporting, analytics) from primary.
- Should be sized with SAME compute/storage capacity as primary — under-sizing risks replication lag.
- Same-Region replication = FREE. Cross-Region = incurs data transfer charges — always default to same-Region unless cross-Region access is specifically required.
- "Scale reads without changing app logic" → Read Replica (same SQL/connection style). ElastiCache/DynamoDB require app code refactoring — wrong if "no logic changes" is stated.

### Aurora Replicas (different from RDS!)
- **Aurora has NO separate "standby" instance concept** — Aurora Replicas serve BOTH purposes: read scaling (via reader endpoint) AND automatic failover target. One mechanism, two benefits.
- Use the **reader endpoint** to spread read load across Aurora Replicas.

### Aurora I/O-Optimized
- Purpose-built for I/O-heavy, spiky workloads. **Flat-rate pricing — no per-I/O-request charges.** Consistent high throughput/low latency. No manual IOPS tuning needed.
- Trigger: "I/O-heavy + unpredictable spikes + no manual tuning + predictable/flat cost" → Aurora I/O-Optimized (not gp2, not io1 — io1 isn't even a real Aurora storage option the way it is for regular RDS).

### Aurora vs DynamoDB
| | Aurora | DynamoDB |
|---|---|---|
| Type | Relational (SQL) | NoSQL (key-value/document) |
| Joins/complex queries | Yes | No |
| Multi-Region writes | NO — single primary Region only (Aurora Global DB is read-only in secondary Regions) | **YES — Global Tables support multi-writer, active-active** |
| Scaling | Vertical + read replicas | Horizontal, massive scale, single-digit ms latency |

**Trigger:** "Every Region needs to WRITE (not just read), globally synchronized, sub-second" → **DynamoDB Global Tables**. If writes only need to happen in ONE central Region → Aurora Global Database is fine.

### IAM Database Authentication
- Generates a **short-lived auth token (15 minutes)**, replaces static password.
- Works the same way regardless of caller (EC2, Lambda, ECS, etc.) — tied to the **IAM role** attached to that compute resource.
- Requires: (1) IAM role attached to the compute service, (2) `rds-db:connect` permission, (3) app code calls SDK method to generate the token each time.
- NOT the same as "Lambda is ephemeral" — a static DB password does NOT expire just because Lambda's execution environment is ephemeral. Only IAM auth tokens are actually time-limited.

### Amazon Neptune
- **Graph database** — nodes + edges, for highly connected data (social networks, recommendation engines, fraud detection, knowledge graphs).
- Query languages: Gremlin, SPARQL, openCypher.

### ElastiCache
- In-memory cache (Redis/Memcached) — a SEPARATE service requiring explicit application code changes (check cache → miss → query DB → write back to cache). NOT automatic.
- Wrong answer whenever "without changing application logic" is stated.

### DynamoDB DAX
- Optional caching layer for DynamoDB — also requires an app code change (switch to DAX client). DynamoDB itself does NOT auto-cache.

---

## 4. NETWORKING

### Load Balancer Types — Critical Protocol Support Table
| LB Type | Layer | Protocols | Static IP? | Special notes |
|---|---|---|---|---|
| **ALB** | 7 (App) | HTTP/HTTPS/WebSocket ONLY | No (DNS name) | Content-based routing, target: EC2/containers/Lambda/IP |
| **NLB** | 4 (Transport) | **TCP AND UDP** | **YES — fixed IP per AZ, or attach EIP** | Only LB supporting UDP; needed for SSH/bastion hosts (SSH=TCP/22) |
| **CLB** | 6/7 (legacy) | Basic HTTP/TCP | No | Legacy, avoid in new designs |

**Golden triggers:**
- "Whitelist a fixed/static IP" (single Region) → **NLB**
- "UDP traffic" → **NLB** (only option — ALB and PrivateLink never support UDP)
- "SSH / bastion host HA" → **NLB** (SSH is TCP)
- "Multiple Regions + need fixed IP" → **AWS Global Accelerator** (2 static anycast IPs, works across Regions) — NLB alone is Region-bound.
- Both ALB and NLB support **TLS/SSL offloading/termination**. CLB supports SSL offloading.

### Route 53 Routing Policies
| Policy | What it does |
|---|---|
| **Simple** | Basic single-record mapping |
| **Weighted** | Split traffic by % across resources |
| **Latency-based** | Route to Region with lowest MEASURED network latency — fully automatic, NO manual zone control |
| **Geolocation** | Fixed manual rules by country/continent (e.g., "Europe → Frankfurt") — no resizing |
| **Geoproximity** | Routes based on geographic distance to resources; **supports "bias"** (-99 to +99) to literally EXPAND/SHRINK the geographic zone routed to a resource |
| **Failover** | Active/passive, uses health checks |
| **Multi-Value** | Returns up to 8 healthy records at random — NOT a scaling mechanism, just basic redundancy/DNS-level distribution |

**Trigger:** "Dynamically alter/resize the SIZE of a geographic area" → **Geoproximity + bias** (the only policy with this exact adjustable-zone capability). Geolocation is the trap (sounds similar, but fixed/manual, no resizing).

### DNS Change Not Taking Effect
- If a Route 53 record is updated but users still hit the OLD endpoint → almost always **TTL caching** (resolvers still serving cached old value). Not a misconfiguration. Fix: lower TTL BEFORE planned changes (recommend 300s).
- **Simple records do NOT support health checks.**

### VPC Connectivity Options
| Need | Solution |
|---|---|
| Secure, LOW bandwidth, QUICK setup, on-prem ↔ AWS | **Site-to-Site VPN** (minutes to set up, IPSec over internet) |
| HIGH bandwidth, dedicated, low-latency, on-prem ↔ AWS, time not a constraint | **Direct Connect** (~1 month to provision) |
| Need MORE than 1.25 Gbps VPN throughput | **Transit Gateway + ECMP + multiple VPN tunnels** (single tunnel caps at 1.25 Gbps; Virtual Private Gateway does NOT support ECMP — common trap) |
| Multiple remote sites need to talk to EACH OTHER (not just to VPC) | **AWS VPN CloudHub** (hub-and-spoke; works with mix of VPN + Direct Connect spokes) |
| Multiple Direct Connect links across MULTIPLE REGIONS + need transitive routing + low ops overhead | **Direct Connect Gateway** (global resource, links DX connections + VGWs across Regions) |
| On-prem needs to reach AWS PUBLIC services (S3, DynamoDB) over Direct Connect, privately | **Public VIF** (Virtual Interface) |
| On-prem needs to reach VPC resources (EC2, RDS) over Direct Connect | **Private VIF** |
| VPC-to-VPC connectivity | **VPC Peering** (NOT transitive — A-B and B-C peering does NOT let A reach C) |

**Key facts:**
- VPC Peering is NEVER transitive.
- Private VIF cannot reach S3 (public service); Public VIF cannot natively reach private VPC resources.
- Transit Gateway + Direct Connect Gateway routes on-prem ↔ VPCs, but does NOT extend to S3 (public service) even with a VPC endpoint inside a connected VPC (that access stays local to that VPC).

### VPC Endpoints Recap
- **Gateway Endpoint**: S3 + DynamoDB ONLY. Free. Route table based.
- **Interface Endpoint**: Everything else (SQS, SNS, Kinesis, etc.). Costs money. ENI-based, ALSO used for on-prem/cross-Region access to S3 (extends beyond Gateway Endpoint's same-VPC-only limitation).

### CloudFront
- **Multi-origin routing**: based on CONTENT TYPE (e.g., static from S3, dynamic from ALB) — NOT based on price class.
- **High availability/failover**: use an **Origin Group** (primary + secondary origin) — NOT geo restriction (that's for blocking by location, unrelated to HA).
- **Field-Level Encryption**: encrypts SPECIFIC fields (max 10) within a request, using a public/private RSA key pair YOU generate (separate from TLS/SSL certs). CloudFront encrypts at the edge using your public key; only the service holding your PRIVATE key (which you manually deploy, e.g. to an EC2 app) can decrypt. Protects sensitive fields (credit card, SSN) from being seen in plaintext by intermediate systems (LB, other backend services) even though TLS terminates and re-establishes at each hop.
- **Restrict S3 to ONLY be accessible via CloudFront** (block direct bucket access): use **Origin Access Identity (OAI)** + update S3 bucket policy to only trust the OAI. (Newer alternative: **OAC — Origin Access Control**, which also supports uploads/writes, unlike OAI which is read-only.)
- **ACM certificate for a custom domain on CloudFront**: MUST be requested in **us-east-1**, regardless of where your content/bucket actually lives. Hard AWS rule.
- **S3 static website endpoints** only support GET/HEAD (read-only) — CANNOT be used as a CloudFront origin for uploads (PUT/POST).
- Signed URLs (single file) / Signed Cookies (multiple files) = actual ACCESS RESTRICTION mechanisms for private content. HTTPS alone only encrypts the connection — does NOT restrict who can access content.

### WAF vs Shield
| | Protects against | Layer |
|---|---|---|
| **WAF** | SQLi, XSS, bad bots, malicious request PATTERNS, rate-based abuse | Layer 7 (application) |
| **Shield Standard** | Common DDoS (free, automatic) | Layer 3/4 |
| **Shield Advanced** | Large-scale/sophisticated DDoS + DRT support + cost protection | Layer 3/4/7 |

- **WAF attaches to exactly 3 places:** CloudFront, ALB, API Gateway. NEVER directly to EC2. NEVER only ALB+API Gateway (CloudFront is also valid — watch for "only" traps).
- **Rate-based rules (block X requests/sec from one source) = a WAF-ONLY feature.** Shield does NOT have rate-based rules — common trap in exam options.
- WAF does much more than block IPs — inspects actual request CONTENT/patterns (SQLi, XSS, rate, geo, size, regex, managed rule groups).

### Security Groups vs Network ACLs
| | Security Group | Network ACL |
|---|---|---|
| Level | Instance (ENI) | Subnet |
| State | Stateful (return traffic auto-allowed) | Stateless (must explicitly allow both directions) |
| Rules | Allow ONLY | Allow AND Deny |
| Evaluation | All rules combined | Ordered (lowest number first), first match wins |

- "Stateful" only auto-allows the RESPONSE to an already-permitted INBOUND request — it does NOT mean all outbound traffic is automatically allowed. New OUTBOUND connections your instance initiates still need their own outbound rule (though default SG outbound = allow all).
- **S3 has NO security groups** — common false/trap answer. Access to S3 is controlled via IAM policies, bucket policies, ACLs — never SGs.
- Need to explicitly block a specific IP → NACL (only NACLs support Deny rules).

---

## 5. SERVERLESS / CONTAINERS

### Lambda
- **Hard max timeout: 15 minutes.** Any job needing longer MUST use something else (Fargate, EC2, Batch) — classic exam trap when a scenario mentions a job duration >15 min alongside "serverless."
- Lambda's execution environment being "ephemeral" has NOTHING to do with database credential rotation — a static DB password doesn't expire just because Lambda restarts.

### ECS vs EKS + Fargate vs EC2
- **"Serverless container orchestration"** = ECS or EKS **+ Fargate** (Fargate is the serverless compute layer, no servers to manage).
- ECS/EKS + **EC2** launch type = valid, but YOU manage the underlying EC2 fleet (patching, scaling) — NOT serverless. Choose EC2 launch type for: cost control at steady high scale, specific instance types (GPU), custom AMIs/kernel modules — trades operational simplicity for control/cost at scale.
- Fargate itself doesn't autoscale on its own — autoscaling is configured at the ECS Service (Application Auto Scaling) or EKS (Horizontal Pod Autoscaler) level, ON TOP of Fargate.

### AWS Batch
- Fully managed batch job scheduling — for jobs that RUN TO COMPLETION (not always-on), too long/heavy for Lambda's 15-min cap, without managing servers.
- Compute environment can use EC2 (including Spot) or Fargate.

---

## 6. MESSAGING & STREAMING

### SQS Feature Cheat Sheet (frequently confused — memorize distinctly)
| Feature | Purpose |
|---|---|
| **Delay Queue** | Postpone delivery of NEW messages before they're first visible to ANY consumer (0s–15min) |
| **Visibility Timeout** | Hide a message from OTHER consumers AFTER it's been picked up by one consumer, while it's being processed (default 30s, max 12hr) |
| **Dead-Letter Queue (DLQ)** | Catches messages that REPEATEDLY FAIL processing, for isolation/debugging |
| **FIFO Queue** | Guarantees message ORDER + exactly-once processing (throughput capped, lower than Standard) |
| **Temporary Queues (Temporary Queue Client / virtual queues)** | High-throughput, low-cost REQUEST-RESPONSE pattern — multiplexes many lightweight "virtual queues" onto ONE real SQS queue, avoiding the cost/overhead of creating many real queues |

### Kinesis Data Streams vs Kinesis Data Firehose
| | Data Streams | Data Firehose |
|---|---|---|
| Latency | TRUE real-time (ms) | Near-real-time (buffered, delivered in batches) |
| Consumers | MULTIPLE simultaneous independent consumers | ONE destination only (S3/Redshift/OpenSearch/Splunk/HTTP) |
| Data retention/replay | Yes (up to 365 days) | No — pass-through only |
| You manage | Must build your own consumer (Lambda/EC2/KCL) | Fully managed, zero consumer code |

**Trigger:** "Multiple applications/consumers need to read the SAME stream" → **Data Streams**. "Just get streaming data into S3/Redshift, no custom processing" → **Firehose**. "Near-real-time OK" → Firehose fine; "true real-time required" → Streams only.

### Amazon MQ
- Managed message broker supporting STANDARD PROTOCOLS: JMS, NMS, AMQP, STOMP, **MQTT**, WebSocket.
- Trigger: "migrate existing on-prem broker WITHOUT changing application logic" + a named standard protocol (MQTT, AMQP, etc.) → **Amazon MQ**. SQS/SNS/Kinesis are AWS-proprietary APIs — would require app rewrites.

### SNS vs SQS vs EventBridge (decoupling)
- **EventBridge**: the ONLY AWS event service integrating directly with THIRD-PARTY SaaS applications. Also ingests 90+ AWS services natively, JSON event rules.
- **SNS**: pub/sub, AWS/internal only, no third-party SaaS integration.
- **SQS**: queuing for decoupling, AWS/internal only, no third-party SaaS integration.
- Trigger: "integrate with third-party SaaS" → **EventBridge**, always.

---

## 7. SECURITY & IAM

### Policy Types
- **Identity-based (user policy)**: attached to IAM user/role/group — governs what that identity can do WITHIN THE SAME ACCOUNT. Cannot grant cross-account access.
- **Resource-based (bucket policy)**: attached to the resource itself (e.g., S3 bucket) — CAN grant access to principals in OTHER AWS accounts. **Cross-account S3 access REQUIRES a bucket policy** — user policies alone can never do this.
- **Permissions boundary**: sets the MAXIMUM permissions an identity can have — never GRANTS access by itself, only caps it.
- **Trust policy**: the ONLY resource-based policy IAM itself supports — attached to an IAM ROLE, defines who can ASSUME the role. (SCPs = AWS Organizations, not IAM. ACLs = service-level like S3/VPC, not IAM.)

### CIDR Notation Quick Reference
- `/32` = exactly ONE specific IP
- `/24` = 256 IPs
- `/0` = entire IP space (everything)
- Smaller number after `/` = BIGGER range. `IpAddress` + `NotIpAddress` together in one condition = "allow this range, EXCEPT this specific exclusion."

### AWS Alternate Contacts (account-level notification routing)
- The AWS-NATIVE mechanism for routing account-level notifications (Billing, Security, Operations) to the correct team — NOT IAM (IAM has no such notification-routing feature).
- Best practice: root account email should be an ALIAS forwarding to a centrally-monitored mailbox (never tied to one individual — single point of failure), PLUS configure Alternate Contacts with team distribution lists for category-specific routing.

---

## 8. DISASTER RECOVERY STRATEGIES (cost vs. speed tradeoff, cheapest→priciest)

| Strategy | Description | Recovery speed |
|---|---|---|
| **Backup & Restore** | Just backups (e.g., to S3), nothing running | Slowest |
| **Pilot Light** | Only the most CRITICAL CORE (e.g., DB) always running; rest built from scratch on failover | Slow-medium |
| **Warm Standby** | SCALED-DOWN but FULLY FUNCTIONAL copy always running; scale up on failover | Fast |
| **Multi-Site (Active-Active)** | FULL-SIZED copy running and actively serving traffic in both locations simultaneously | Fastest, most expensive |

**Trigger phrase match:** "Scaled-down but fully functional, always running" = **Warm Standby**, exactly.

---

## 9. COST OPTIMIZATION TOOLS — Precise Scope (easy to mix up)

| Tool | What it actually covers |
|---|---|
| **Cost Optimization Hub** | Consolidated recommendations: rightsizing, idle resource deletion, Savings Plans, RIs — across accounts/Regions |
| **Compute Optimizer** | ONLY EC2 instance TYPE/sizing recommendations (based on utilization) — does NOT recommend purchasing options (RIs/Savings Plans) |
| **S3 Storage Class Analysis** | ONLY Standard → Standard-IA recommendations — NOT Glacier or One Zone-IA |
| **Trusted Advisor** | Flags/notifies (e.g., RIs expiring soon) — does NOT auto-renew or auto-fix anything |
| **S3 Storage Lens** | Org-wide S3 usage/config METRICS and reporting (not a cost-recommendation engine per se, but supports compliance/cost visibility) |

### Savings Plans Scope
| Plan | Covers |
|---|---|
| **Compute Savings Plan** | EC2 + Fargate + Lambda (broadest, NOT tied to instance family/Region) — does NOT cover SageMaker |
| **EC2 Instance Savings Plan** | EC2 ONLY, tied to instance family/Region — does NOT cover Fargate |
| **SageMaker Savings Plan** | SEPARATE, dedicated plan required for SageMaker (training/inference/notebooks) — never bundled into Compute or EC2 plans |

**Trigger:** Mixed workload across EC2+Fargate+Lambda+SageMaker, fewest plans/broadest coverage → **Compute Savings Plan + SageMaker Savings Plan** (2 plans covers everything).

---

## 10. MIGRATION & DATA TRANSFER

| Need | Tool |
|---|---|
| One-time BULK migration (TB–PB scale) from on-prem | **AWS DataSync** (fast, automated, up to 10x faster than CLI tools) |
| Ongoing/ACCESS to migrated data from on-prem apps afterward | **Storage Gateway — File Gateway** (SMB/NFS access to S3, local caching) |
| Massive one-time transfer (PB scale), no network transfer feasible | **Snowball / Snowball Edge** (physical device; data lands in S3 first, THEN lifecycle-transition to Glacier — cannot go Snowball→Glacier directly) |
| Speed up direct client-to-S3 UPLOADS over long distances | **S3 Transfer Acceleration** (uses CloudFront edge locations) — NOT for ongoing access or migration orchestration |
| S3-to-S3, same-Region or cross-Region, ONE-TIME copy of EXISTING objects | **`aws s3 sync`** CLI, or **S3 Batch Replication** (for pre-existing objects; delete config after) — regular/live replication only covers NEW objects going forward |

**Pattern:** DataSync = migrate. File Gateway = keep using afterward. Never confuse Transfer Acceleration (upload speed) with a migration/access solution.

---

## 11. ANALYTICS / DATA PROCESSING

| Need | Service |
|---|---|
| Audio → text | **Amazon Transcribe** |
| Ad-hoc SQL queries on data in S3 | **Amazon Athena** |
| Data visualization/dashboards (NOT a SQL query tool) | **Amazon QuickSight** |
| ETL, data cataloging, schema discovery, "glue" between raw data and analytics tools | **AWS Glue** (Crawlers = auto schema discovery; Data Catalog = central metadata; Jobs = serverless Spark ETL) |
| Real-time streaming ingestion + processing, MULTIPLE consumers | **Kinesis Data Streams + Lambda** |

---

## 12. HIGH-LEVEL PATTERN RECOGNITION CHEAT SHEET

| Phrase in the question | Points to |
|---|---|
| "Whitelist a public/static IP" (single Region) | NLB |
| "Whitelist IP" across MULTIPLE Regions | Global Accelerator |
| "Dynamically resize a geographic routing area" | Route 53 Geoproximity + bias |
| "UDP traffic" | NLB (only LB supporting UDP) |
| "Tightly coupled, low-latency HPC" | Cluster Placement Group (+ EFA if networking device asked) |
| "Avoid correlated failures" | Spread Placement Group |
| "Server-bound software license" | Dedicated HOSTS (not Instances) |
| "Single-tenant hardware, cost matters" | Dedicated INSTANCES (not Hosts) |
| "Temporary/scratch storage, high IOPS, cheap" | Instance Store |
| "Guaranteed/consistent IOPS" | io1/io2 |
| "Under-utilized + occasional bursts, reduce cost" | Switch io1 → gp2 |
| "Multi-Attach" | io1/io2 ONLY, same AZ, never root volume |
| "Scale reads, no app logic changes" | RDS Read Replica |
| "Multi-AZ standby serve reads" | NEVER possible — standby is failover-only |
| "Every Region writes, active-active" | DynamoDB Global Tables |
| "I/O-heavy, unpredictable, no manual tuning, flat cost" | Aurora I/O-Optimized |
| "Short-lived/rotating DB credentials" | IAM Database Authentication (15-min tokens) |
| "Third-party SaaS integration" | EventBridge |
| "Migrate broker, keep app logic, MQTT/AMQP" | Amazon MQ |
| "Multiple consumers read same stream" | Kinesis Data Streams |
| "Simple delivery to S3/Redshift, no consumer code" | Kinesis Data Firehose |
| "Job runs >15 minutes, still serverless" | Fargate (not Lambda) |
| "Serverless container orchestration" | ECS/EKS + Fargate |
| "Proprietary/custom encryption algorithm" | S3 Client-Side Encryption |
| "Reduce KMS cost, keep using KMS" | S3 Bucket Keys |
| "Cross-account S3 access" | Bucket Policy (never user policy alone) |
| "Restrict S3 to CloudFront only" | OAI (or OAC for read+write) |
| "ACM cert for CloudFront" | Must be us-east-1, always |
| "Rate-based rule / X requests per second" | WAF (never Shield — Shield has no rate rules) |
| "SQL injection / XSS / bad bots" | WAF |
| "DDoS, volumetric attack" | Shield (Standard/Advanced) |
| "Block a specific IP at subnet level" | Network ACL (not Security Group) |
| "S3 + security group" | INVALID — S3 has no security groups |
| "Bastion host + HA" | NLB (SSH = TCP) |
| "On-prem multiple sites talk to EACH OTHER" | AWS VPN CloudHub |
| "On-prem reach S3 via Direct Connect" | Public VIF (not Private VIF) |
| "VPC-to-VPC, not transitive" | VPC Peering (remember: never transitive) |
| "Need >1.25 Gbps VPN throughput" | Transit Gateway + ECMP (not Virtual Private Gateway — no ECMP support) |
| "Immediate access always required" | Rules out ALL Glacier tiers |
| "Critical/hard to reproduce data" | Rules out One Zone-IA |
| "Unpredictable access pattern" | S3 Intelligent-Tiering |
| "Scaled-down but fully functional, always running" (DR) | Warm Standby |
| "AMI copy across Regions" | Required — AMIs are Region-specific |
| "Logs must survive instance termination" | CloudWatch Logs agent |
| "Ready-only file for a SaaS integration, standby unused" | N/A — Multi-AZ standby never queryable, period |
