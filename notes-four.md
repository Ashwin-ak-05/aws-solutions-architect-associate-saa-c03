# AWS Solutions Architect Associate — Session Study Notes

A concept-based reference organized by service/topic. Focus is on **when to use what, and why** — the decision triggers you should recognize on the exam.

---

## 1. Amazon S3

### 1.1 Storage Classes — Use Cases

| Storage Class | Use Case | Availability | AZ Spread |
|---|---|---|---|
| **S3 Standard** | Frequently accessed, active data | 99.99% | Multi-AZ |
| **S3 Intelligent-Tiering** | Unknown/changing access patterns; auto-moves objects between tiers, no retrieval fee, small monitoring fee | 99.9% | Multi-AZ |
| **S3 Standard-IA** | Infrequently accessed but needs millisecond retrieval (known/predictable access pattern) | 99.9% | Multi-AZ |
| **S3 One Zone-IA** | Infrequent access + **re-creatable** data; ~20% cheaper than Standard-IA | 99.5% | **Single AZ** |
| **S3 Glacier Instant Retrieval** | Archive, accessed ~quarterly, millisecond retrieval needed | Multi-AZ |
| **S3 Glacier Flexible Retrieval** | Archive, accessed 1-2x/year, minutes-hours retrieval | Multi-AZ |
| **S3 Glacier Deep Archive** | Long-term (7-10yr) compliance archival, cheapest, hours retrieval | Multi-AZ |

**Decision rule:** "re-creatable" data → One Zone-IA. "Known access pattern" → Standard-IA (cheaper than Intelligent-Tiering, which is for *unpredictable* patterns). "Millisecond latency required" → never Glacier tiers.

**Minimum storage duration:** 30 days before transitioning OUT of S3 Standard. Objects should be ≥128KB for IA transitions to be cost-effective.

### 1.2 Valid vs Invalid Lifecycle Transitions

**Valid ("waterfall" — always gets colder/cheaper):**
- Standard → anything
- Standard-IA → Intelligent-Tiering, One Zone-IA, Glacier tiers
- Intelligent-Tiering → One Zone-IA, Glacier tiers (NOT Standard-IA)
- One Zone-IA → Glacier tiers ONLY (dead end otherwise)
- Anything → Glacier Instant/Flexible Retrieval/Deep Archive

**Invalid (memorize — common trap):**
- ❌ Anything → S3 Standard (never goes back)
- ❌ Anything → Reduced Redundancy (deprecated)
- ❌ Intelligent-Tiering → Standard-IA
- ❌ One Zone-IA → Standard-IA or Intelligent-Tiering

### 1.3 S3 Object Lock / Retention Periods

- **Explicit retention** on an object version → you set a `Retain Until Date` directly.
- **Bucket default setting** → you set a **duration** (days/years), NOT a date; S3 calculates the actual date per object.
- **Explicit retention always overrides bucket default** (not the other way around).
- Retention is **per object VERSION** — different versions of the same object key can have completely different retention settings.

### 1.4 S3 Performance / Scaling (Request Rate Issues)

- S3 auto-scales to **at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests/sec PER PREFIX**.
- **No limit on number of prefixes** in a bucket.
- **Fix for request-rate throttling** → add more **prefixes** within the SAME bucket (e.g., `bucket/customer123/file1`), NOT new buckets per customer/day (wastes globally-unique bucket names) and NOT switching to EFS (more expensive).

### 1.5 S3 Transfer Acceleration (S3TA)

- Uses CloudFront edge locations to route uploads over AWS's optimized network path.
- **Pricing: pay only if acceleration actually happens.** If S3TA doesn't speed things up, you pay $0 for that transfer (and inbound transfer to S3 is ALWAYS free regardless of S3TA).
- **Fastest way to speed up S3 uploads for distant users:** S3 Transfer Acceleration + Multipart Uploads (for files >100MB) — NOT Direct Connect (takes months, overkill) or VPN (no speed benefit) or Global Accelerator (that's for compute endpoints like ALB/NLB/EC2, not S3).

### 1.6 S3 Encryption Options

| Type | Who holds/manages the key | Audit trail (CloudTrail)? |
|---|---|---|
| **SSE-S3** | AWS internally, silently | ❌ No |
| **SSE-C** | Customer provides key per-request; S3 never stores it | ❌ No |
| **SSE-KMS** | AWS KMS holds a Customer Master Key (CMK) you own/manage | ✅ **Yes** |
| **Client-side encryption** | Customer encrypts before upload | N/A — customer manages entirely |

**Rule:** Need "no key management burden + audit trail of who used the key" → **SSE-KMS**. Every KMS `Encrypt`/`Decrypt`/`GenerateDataKey` call is a loggable API call, which is why CloudTrail can show usage — S3's own internal keys (SSE-S3) or customer-supplied keys (SSE-C) never go through a trackable KMS API call.

**How SSE-KMS actually works (envelope encryption):** S3 asks KMS to generate a unique **data key** per object → KMS returns a plaintext + CMK-encrypted version → S3 uses plaintext version to encrypt the object, discards it, stores only the encrypted data key as metadata. On read, S3 sends the encrypted data key back to KMS to unwrap it — *this* unwrap request is what gets logged.

**Why SSE-KMS ≠ SSE-C conceptually:** In SSE-KMS you *request* a key to exist, but AWS KMS generates/holds the actual key material in secure hardware — you get a separate, controllable **key policy** (a second access-control gate beyond your S3/IAM permissions). In SSE-C, you generate and hold the key yourself.

### 1.7 IAM Policy — Bucket vs Object ARNs (common trap)

- **Bucket-level actions** (e.g. `s3:ListBucket`) → apply to `arn:aws:s3:::bucket-name`
- **Object-level actions** (e.g. `s3:GetObject`, `s3:PutObject`, `s3:DeleteObject`) → apply to `arn:aws:s3:::bucket-name/*`

Mixing these up (e.g., applying `DeleteObject` to the bucket ARN without `/*`) silently fails.

---

## 2. AWS Storage Gateway (Hybrid Storage)

| Type | Protocol/Interface | Best for |
|---|---|---|
| **File Gateway** | NFS or SMB, backed by S3 | On-prem apps needing to keep using NFS/SMB while storing data in S3 with lifecycle tiering |
| **Volume Gateway (cached mode)** | iSCSI block storage | Block-level access; snapshots to S3/Glacier |
| **Tape Gateway** | Virtual Tape Library (VTL) | Replacing physical backup tape infrastructure |

**Key exam trap:** "Keep using NFS, minimize changes, need automated tiering" → **File Gateway**, NOT Volume Gateway (that's block storage, awkward to mount NFS on top of) and NOT EFS (requires full migration, not hybrid) and NOT FSx for Windows (that's SMB-only, requires app changes).

---

## 3. EC2 — Purchasing Options & Storage

### 3.1 On-Demand vs Spot vs Reserved

| | On-Demand | Spot | Reserved |
|---|---|---|---|
| Cost | Highest baseline | Up to 90% cheaper | Cheaper than On-Demand, requires commitment |
| Availability | Guaranteed | Can be reclaimed (2-min warning), <10% avg interruption rate | Guaranteed |
| Commitment | None | None | 1 or 3 years |
| Best for | Can't tolerate interruption, unpredictable workloads | **Fault-tolerant, flexible start/stop, interruptible** workloads (batch, CI/CD, rendering, HPC) | Steady-state, predictable, long-term workloads |

**Decision cue:** "can withstand disruption," "start/stop multiple times," "flexible" → **Spot**. "Guaranteed availability, no interruption" → On-Demand. "Long-term, steady, 1-3yr horizon" → Reserved. **Never use Reserved for short/flexible workloads** (locked into term).

### 3.2 Instance Store vs EBS

| | Instance Store | EBS |
|---|---|---|
| Attachment | Physically attached to host | Network-attached |
| Performance | Highest raw I/O (no network hop) | Very good, some network latency |
| Persistence | **Ephemeral** — lost on stop/terminate/host failure (survives reboot) | Persistent |
| Cost | Included in instance price | Billed separately |
| Resizable/detachable | No | Yes |
| Boot volume? | Some instance types support it | Yes, standard |

**When to use Instance Store:** App-level replication already provides resilience (e.g., data replicated by the app across a fleet), and you need max I/O at lowest cost — the ephemeral nature is acceptable because the app handles instance loss. Cheaper AND faster than Provisioned IOPS (io1) EBS for this scenario.

**EBS volume types that CANNOT be boot volumes:** **st1** (Throughput Optimized HDD) and **sc1** (Cold HDD) — both are HDD-backed, optimized for large sequential throughput, not bootable. SSD-backed (gp2/gp3, io1/io2) CAN be boot volumes. Instance Store can also be a boot volume for supported instance types.

### 3.3 Placement Groups

| Type | Purpose | Behavior |
|---|---|---|
| **Cluster** | Low latency, high throughput (HPC, tightly-coupled node communication) | Packs instances close together, single AZ |
| **Spread** | Avoid correlated hardware failures | Instances on distinct racks/hardware, max 7/AZ/group, can span AZs |
| **Partition** | Large distributed/replicated workloads (Hadoop, Cassandra, Kafka) | Groups isolated into partitions (max 7/AZ), can span AZs |

**Decision cue:** "HPC," "tightly-coupled," "low-latency node-to-node" → **Cluster**. "Minimize correlated failure risk for critical isolated instances" → **Spread**. "Large distributed replicated system" → **Partition**.

---

## 4. Auto Scaling Groups (ASG)

### 4.1 Scaling Policy Types

| Policy | Mechanism | Use when |
|---|---|---|
| **Target Tracking** | Set a metric + target value (e.g., CPU=50%); AWS auto-creates/manages the CloudWatch alarms and continuously adjusts capacity | "Maintain X% utilization" / "keep metric near a target value" |
| **Step Scaling** | You define specific CloudWatch alarm thresholds + step adjustments | Reactive, multi-tier thresholds |
| **Simple Scaling** | You define one alarm threshold + a single scaling action + cooldown | Basic reactive scaling (no "target value" concept exists here at all) |
| **Scheduled Action** | Triggers at a specific date/time; set **desired capacity** (not min/max, unless you also want to change the range) | **Predictable, calendar-based** traffic (e.g., "last day of the month at 5pm") |

**Key distinction:** Simple/Step scaling have NO concept of a "target value" — if a question says "set CPU as target metric with target value of X%," that phrasing ONLY applies to **Target Tracking**. Scheduled Actions are the ONLY policy type that reacts to **calendar time**, not real-time metrics.

**Desired Capacity vs Min/Max:** Use **desired capacity** when you want an exact number of instances. Use **min/max** only when defining an allowable *range*.

### 4.2 ASG Process Types & Maintenance Patterns

To patch a specific instance WITHOUT the ASG replacing it during the temporary unhealthy window, use ONE of:
- **Standby state** — instance stays part of ASG, but stops receiving traffic and isn't touched by health checks. Patch, then exit Standby.
- **Suspend the `ReplaceUnhealthy` process type** — pauses the specific "detect unhealthy → terminate → replace" behavior for the whole ASG. Patch, mark healthy manually, then resume.

(NOT: creating new AMI + new instance = wasteful; NOT deleting/recreating the ASG = overkill; NOT suspending `ScheduledActions` = wrong process, that controls scheduled scaling, not health-check replacement.)

### 4.3 AZ Rebalancing vs Unhealthy Instance Replacement (opposite sequencing!)

| Scenario | Sequence |
|---|---|
| **AZ Rebalancing** (e.g., manual termination left AZs unbalanced) | **Launch new instances FIRST, then terminate old ones** (protects availability) |
| **Unhealthy instance replacement** (failed health check) | **Terminate FIRST (separate scaling activity), THEN launch replacement** (separate scaling activity) — never simultaneous |

---

## 5. Elastic Load Balancing

| | Application Load Balancer (ALB) | Network Load Balancer (NLB) |
|---|---|---|
| OSI Layer | Layer 7 (HTTP/HTTPS) | Layer 4 (TCP/UDP) |
| Content-based routing | ✅ Yes (path, host, headers) | ❌ No |
| Best for | Microservices, content-based routing, modern app architectures | Extreme performance, static IP, TCP/UDP protocols |

**Key trap:** ASGs **do not distribute traffic** — that's always the load balancer's job. ASGs only manage instance count/health.

---

## 6. Networking / Hybrid Connectivity

### 6.1 AWS Direct Connect

- Dedicated, private, physical fiber connection from on-prem to AWS (via a Direct Connect location).
- **NOT encrypted by default.**
- **Takes months to provision** (physical cross-connects, partner coordination, port availability, LOA-CFA process) — NOT for quick/one-time needs.
- Use when: sustained high-volume transfer, latency/consistency-sensitive apps, long-term hybrid architecture, large one-time migrations, compliance requiring private connectivity.
- **Direct Connect + VPN** = combines Direct Connect's performance with VPN's IPsec encryption — the answer whenever a question demands **dedicated + encrypted + low latency + high throughput** ALL together.

### 6.2 Site-to-Site VPN

- IPsec-encrypted connection over the **public internet**.
- Fast to set up, low/modest bandwidth, tolerates internet variability.
- Cannot guarantee low latency/high throughput (rules it out when performance is a hard requirement).

### 6.3 VPC Peering

- Direct, private connection between **exactly two VPCs** — traffic stays on AWS's private backbone.
- **NOT transitive** (A↔B and B↔C does NOT give A↔C).
- Works: same/different account, same/different Region (inter-Region peering).
- **Non-overlapping CIDR ranges required.**
- A VPC is scoped to **one account + one Region** — a single account CAN have multiple VPCs (default quota ~5/Region, increasable), and separate VPCs (even same Region) need explicit peering/Transit Gateway to connect.

### 6.4 AWS Transit Gateway

- Central hub connecting many VPCs + on-prem networks — avoids full-mesh peering complexity (N connections instead of N×N).
- By itself does NOT create a dedicated/encrypted connection to on-prem — must be paired with VPN or Direct Connect for that link; TGW then handles routing.
- **Cannot use VPC Peering AND Transit Gateway together for the same VPC pair** — architectural contradiction, pick one.

### 6.5 AWS PrivateLink

- Private, secure connectivity to a **specific service** (not a whole network) across accounts/VPCs — no internet gateway, NAT, VPN, or Direct Connect required.
- Uses an **Interface VPC Endpoint** (ENI with private IP in your VPC) to reach a **VPC Endpoint Service** (typically fronted by an NLB) in the provider's account.
- No CIDR overlap restrictions (unlike peering) — because it's not full network routing, just point-to-point service access.
- **RDS cannot be exposed directly via PrivateLink** — must front it with an NLB (+ optionally RDS Proxy) first.
- **Use when:** cross-account access needed, VPC has NO internet/VPN/Direct Connect, and only a *specific service* (not broad network access) needs to be reached.

### 6.6 AWS Global Accelerator

- Uses AWS's global network + anycast static IPs to route users to the nearest edge, then travels over AWS's private backbone to the healthy endpoint (ALB/NLB/EC2) — reduces latency/jitter, improves throughput vs public internet.
- Works for **TCP AND UDP** (unlike CloudFront which is HTTP/HTTPS only).
- Provides **fast regional failover** at the network/proxy layer — NOT dependent on DNS propagation/TTLs (unlike Route 53 failover).
- **Use when:** non-HTTP protocols (gaming/UDP, VoIP, IoT/MQTT), need static IPs, need fast deterministic regional failover, want to keep your OWN custom DNS (Global Accelerator doesn't require using Route 53).
- Different from ELB: ELB load balances **within one Region**; Global Accelerator manages traffic **across Regions** (and can use a regional ELB as its target).

### 6.7 CloudFront

- CDN for **HTTP/HTTPS** content (static + dynamic), uses edge locations to cache/accelerate.
- Supports a **custom origin** — the origin does NOT have to be in AWS; it can be an **on-premises HTTP server**. Great for improving global latency for a dynamic on-prem website WITHOUT migrating the backend.
- CloudFront does **not run "inside" a VPC** — it's a separate global edge network.
- **Geo Restriction** feature — whitelist/blacklist countries for content access via CloudFront.

### 6.8 Geographic / Country-based Access Control

| Need | Tool |
|---|---|
| Restrict S3/CloudFront-distributed content by country | **CloudFront Geo Restriction** |
| Restrict ALB-fronted application traffic by country | **AWS WAF** with **Geo Match Conditions** on the ALB |
| Restrict distribution to only DNS queries from certain locations | **Route 53 Geolocation routing policy** |

**Security Groups can NEVER do geo-blocking** — they only filter by IP/port/protocol, not geographic origin. This is a very common trap.

### 6.9 Route 53 Routing Policies (recap)

| Policy | Purpose |
|---|---|
| **Geolocation** | Route based on user's geographic location; can restrict content to specific regions (distribution-rights use case) |
| **Latency-based** | Route to the Region with lowest latency |
| **Weighted** | Split traffic by assigned percentages (load balancing, A/B testing) |
| **Failover** | Route to backup resource only if primary is unhealthy |
| **Geoproximity** | Route based on geographic distance, with bias adjustment |

### 6.10 Route 53 Resolver (Hybrid DNS)

- **Outbound Endpoint + forwarding rule** → lets **AWS VPC resources resolve ON-PREMISES private DNS names** (query goes OUT from VPC → on-prem DNS server via VPN/Direct Connect).
- **Inbound Endpoint** → lets **on-premises systems resolve AWS-hosted private DNS names** (query comes IN to Route 53) — the opposite direction.
- **Private Hosted Zone** → for domains Route 53 itself is authoritative for (AWS-hosted); does NOT proxy to an actual external DNS server — you'd have to manually duplicate on-prem records (bad/stale).

**Decision cue:** "AWS app needs to resolve on-prem domain names" → Outbound Endpoint. "On-prem needs to resolve AWS private domain names" → Inbound Endpoint.

---

## 7. IAM & Identity

### 7.1 IAM Best Practices (exam checklist)

- ✅ Individual accounts per user — **never share credentials**.
- ✅ Least privilege — **never grant maximum/excess permissions "to avoid reassigning later."**
- ✅ Use **IAM Roles** for EC2/service access — never embed long-term user credentials in an app.
- ✅ Enable **MFA** for privileged users.
- ✅ Enable **CloudTrail** to log all IAM actions (audit).

### 7.2 IAM Roles vs Users for Cross-Account Access

- Cross-account resource access → always **IAM Roles** (via `sts:AssumeRole`), never shared IAM user credentials.
- Roles issue **temporary credentials** via AWS STS (Access Key, Secret Key, Session Token + expiration, typically 15min–12hrs). Auto-expire; must re-assume for continued access.
- IAM user access keys are **static/long-term** — valid indefinitely until manually rotated/deleted. This is why roles are the security best practice for delegation.

### 7.3 Hybrid Identity: AD Connector vs AWS Managed Microsoft AD

| | AD Connector | AWS Managed Microsoft AD |
|---|---|---|
| What it is | **Proxy/gateway** — no directory data stored in AWS | A real, separate directory running IN AWS |
| Data | None — always queries on-prem AD directly | Stores its own data (synced via trust relationship) |
| Overhead | Low (no directory to manage) | Higher (manage trust, directory health) |
| Requires | VPN or Direct Connect to reach on-prem AD | Trust relationship setup |

**Best pattern for "multi-account, on-prem AD, centralized, low operational overhead":** **AD Connector + IAM Identity Center** — AD Connector proxies auth to on-prem AD, IAM Identity Center uses AD group memberships to assign **permission sets** across AWS accounts (Organizations), giving SSO + centralized access management with minimal maintenance.

**IAM Identity Center** = the central hub/control plane (permission sets, SSO portal, multi-account access). It supports MULTIPLE identity sources: AD Connector, AWS Managed Microsoft AD, its own built-in directory, or **external SAML 2.0 IdPs** (Okta, Azure AD/Entra ID, Ping, OneLogin) — often with SCIM for automatic user/group provisioning.

### 7.4 RDS Custom vs Standard RDS (OS-level access)

| | Standard RDS | RDS Custom |
|---|---|---|
| Host OS access | ❌ Never (fully opaque) | ✅ SSH/RDP access to underlying EC2 host |
| Use case | Standard managed DB workloads | Legacy apps needing custom OS/DB patches, specific configs, third-party agent software |
| HA | Multi-AZ available | Multi-AZ available |

**Decision cue:** "Need DBA-level OS/database customization" + "minimize maintenance" + Oracle/SQL Server → **RDS Custom in Multi-AZ**. Self-managed EC2 gives full control but full operational burden — RDS Custom is the managed middle ground.

---

## 8. RDS / Aurora

### 8.1 Multi-AZ vs Read Replicas

| | Multi-AZ | Read Replica |
|---|---|---|
| Replication | **Synchronous** | **Asynchronous** |
| Purpose | High Availability / Durability (failover) | Read scalability / performance |
| Spans | At least 2 AZs, single Region | Same AZ, Cross-AZ, or **Cross-Region** |
| Readable under normal ops? | No (standby is passive failover target) | Yes (actively queryable) |

**Synchronous = write isn't confirmed to the app until BOTH primary and standby have durably written it** — guarantees zero data loss on failover, at the cost of slightly higher write latency. Async Read Replicas confirm the write immediately after the primary persists it, so there can be a small replication lag.

### 8.2 Aurora Global Database

- Single Aurora database spanning **multiple Regions** — for globally distributed relational data needing fast local reads + DR.
- **Use when app already runs on Aurora and minimal refactoring is required** — keeps everything in the SQL/relational ecosystem. Switching any table to DynamoDB (NoSQL, different API) for "global access" would require MORE refactoring, even if technically it could work — always stay within the existing DB paradigm when "minimal refactoring" is stated.

### 8.3 Babelfish for Aurora PostgreSQL

- Lets Aurora PostgreSQL **understand T-SQL syntax and SQL Server wire protocol** — apps built for SQL Server can talk to Aurora PostgreSQL with minimal/no query rewriting.
- Migration toolset: **AWS SCT** (Schema Conversion Tool — converts schema) + **AWS DMS** (Database Migration Service — migrates the actual data).
- **Full pattern for "SQL Server → Aurora PostgreSQL, minimal app changes":** Babelfish (query compatibility) + SCT/DMS (schema + data migration).

---

## 9. File/Storage Systems: EFS vs FSx

### 9.1 EFS (Elastic File System)

- **Protocol:** NFS only (Linux/Unix)
- **Region-scoped**, but **Multi-AZ by default** (Standard class) — data redundantly stored across AZs automatically (no config needed, unlike RDS Multi-AZ's primary/standby model — EFS's AZs are all *active* simultaneously, more like S3's replication model).
- **One Zone / One Zone-IA** = single-AZ, cheaper, less resilient (for re-creatable/non-critical data).
- **Cross-Region access:** not a single unified filesystem; achieved via **inter-Region VPC Peering** connecting EC2 in other Regions to the mount targets (real-time shared access), OR **EFS Replication** (async, creates a separate read-only replica — for DR, not live collaboration).
- **Cross-account access:** EFS resource policy + VPC peering/shared VPC + EFS Access Point (native support, no data duplication).
- **Access control:** VPC Security Groups (network layer, instance-level) + IAM policies (who can mount + permissions) + POSIX permissions (file/directory level). Network ACLs do NOT work here (subnet-level, not instance-level — too coarse). GuardDuty is NOT an access control tool (it's threat detection).

### 9.2 FSx Family (purpose-built file systems)

| Type | Protocol | Best for |
|---|---|---|
| **FSx for Windows File Server** | SMB | Windows workloads, AD integration, DFS support |
| **FSx for Lustre** | Lustre (POSIX) | HPC, ML training, video rendering, genomics, chip design — extreme parallel throughput; **native S3 integration** (presents S3 objects as files, writes back to S3) |
| **FSx for NetApp ONTAP** | NFS/SMB/iSCSI | Migrating from on-prem NetApp, need dedup/cloning/ONTAP snapshots |
| **FSx for OpenZFS** | NFS | Migrating from on-prem ZFS, high IOPS/low latency |

**Key fact:** FSx for Windows does NOT support Microsoft DFS in the same folder-structure sense some questions test — actually it DOES support DFS (correction: FSx for Windows File Server DOES support DFS for organizing shares up to hundreds of PB). FSx for Lustre does NOT support DFS.

### 9.3 EFS vs FSx for Lustre — Cost/Performance framing

- Both can be Linux/POSIX — the differentiator is NOT the OS, it's **architecture and performance profile**.
- **EFS** = general-purpose distributed NFS, good for many small/medium files, moderate throughput, **cheaper** ($/GB).
- **FSx for Lustre** = purpose-built parallel HPC file system, extreme aggregate throughput across many compute nodes simultaneously, **more expensive** ($/GB) because it's dedicated high-performance provisioned infrastructure.
- **Don't compare purely on $/GB** — if Lustre's speed dramatically cuts processing/compute time (fewer EC2-hours), it can be more cost-**effective** overall despite higher storage unit cost.

---

## 10. Containers & Serverless Compute

### 10.1 ECS: EC2 Launch Type vs Fargate Pricing

| | EC2 Launch Type | Fargate Launch Type |
|---|---|---|
| Billed for | EC2 instances + EBS volumes used | vCPU + memory the task actually requests |
| Server management | You manage instances | Serverless, no infra to manage |

### 10.2 EKS — Pod-Level Least Privilege (IRSA)

- **IAM Roles for Service Accounts (IRSA)** = the AWS-recommended way to give individual Kubernetes Pods fine-grained AWS permissions.
- Create **separate Kubernetes service accounts** per workload type, map each to its **own scoped IAM role** via IRSA (uses EKS's OIDC provider).
- **Traps:** IAM policies CANNOT be attached directly to Pods via annotations (not a real mechanism). A single shared service account with combined permissions defeats least privilege even if IRSA is used. Node-level EC2 instance profile IAM policies give ALL Pods on that node the same broad permissions — Kubernetes RBAC controls K8s API access, NOT AWS service permissions.

### 10.3 When to use Lambda vs ECS/Fargate vs EC2

- **Lambda** → short, event-driven, lightweight logic (e.g., validation step). Hard limit: **15 minutes max execution**.
- **ECS on Fargate** → longer-running, compute/memory-intensive backend processing, still fully serverless (no cluster/OS management).
- **EC2 (self-managed/EKS self-managed nodes)** → only when the question explicitly needs OS-level control; otherwise it's a red flag for "minimize operational overhead" requirements.

**Common architecture pattern for "lightweight validation → heavier backend, fully managed, minimal overhead":** API Gateway → Lambda (validation) → ECS/Fargate (backend).

### 10.4 AWS Outposts + EKS Anywhere

- **Outposts** brings actual AWS hardware/services (EKS, EC2, S3, CloudWatch, IAM) physically into your own data center.
- **EKS Anywhere on Outposts** = run Kubernetes with full AWS API integration (auto upgrades, CloudWatch, IAM) while ALL data/workloads stay 100% on-premises — the answer when compliance mandates zero data leaving the premises but you still want modern AWS tooling.
- Contrast: Direct Connect/Local Zones/Transit Gateway still put workloads IN an AWS-controlled facility (violates strict on-prem residency). Snowball Edge is for temporary/edge/offline transfer, not persistent production Kubernetes.

---

## 11. Messaging & Streaming

### 11.1 SQS Standard vs FIFO

| | Standard | FIFO |
|---|---|---|
| Ordering | Best-effort | **Strict order guaranteed** |
| Delivery | At-least-once (possible duplicates) | **Exactly-once** |
| Default throughput | Nearly unlimited | 300 msg/sec (send/receive/delete ops) |
| With batching | N/A | Up to 3,000 msg/sec (10 msgs/operation, max batch) |

**Throughput math:** ops/sec (300) × messages-per-batch = effective throughput. Pick smallest batch size that clears the required rate.

**Decision cue:** "exactly once" + "in order" → **FIFO** (with batching if throughput demands it). Never Standard or EventBridge for strict ordering/exactly-once (both are at-least-once, best-effort order).

### 11.2 SNS vs EventBridge vs SQS vs Kinesis

| Service | Model | Ordering | Delivery guarantee |
|---|---|---|---|
| **SNS** | Pub/sub | No | At-least-once |
| **EventBridge** | Event bus, rules-based routing | No | At-least-once |
| **SQS Standard** | Queue | Best-effort | At-least-once |
| **SQS FIFO** | Queue | Strict | Exactly-once |
| **Kinesis Data Streams** | Streaming, ordered by shard/partition key | **Yes, preserves order** | Real-time, replayable |

**Kinesis Data Firehose** — CANNOT write directly to DynamoDB (common factual trap). Firehose destinations: S3, Redshift, OpenSearch, Splunk, certain HTTP/3rd-party endpoints only.

### 11.3 Serverless Ordered-Processing Pattern (gaming leaderboard example)

**"Handle traffic spikes + process in order + store in HA DB + minimize overhead"** → **Kinesis Data Streams → Lambda (native integration, handles polling/checkpointing) → DynamoDB**. Never use EC2 for the processing layer if "minimize overhead" is a requirement — always disqualifies EC2-based consumer options even if paired with the right ingestion/storage services.

### 11.4 Fully Serverless Ingestion Pattern (no manual capacity provisioning)

**"Fully serverless, no manual capacity provisioning"** → **SQS → Lambda (polls in batches) → DynamoDB (auto-scaled)**. Any option with an EC2 instance in the pipeline is disqualified regardless of how good the other components are.

### 11.5 Lambda Concurrency Quota (troubleshooting dropped messages)

- Default: **1,000 concurrent executions per account per Region.**
- If SNS→Lambda (or any high-volume trigger) exceeds this, Lambda **throttles**, and messages get dropped — NOT because SNS or Lambda "hit a scalability limit" (both are fully managed/auto-scaling) — it's specifically the **account concurrency quota**.
- Fix: **contact AWS Support to raise the limit** (soft quota) — you cannot "add more servers" to serverless services.

### 11.6 Decoupling Pattern for Write-Heavy DB Timeouts

**"App times out under peak write load, DB layer can't be re-engineered, need scalable + cost-effective fix":**
- **SQS + Auto Scaling EC2 workers** — decouples web tier from processing, buffers bursts, elastic compute.
- **RDS Proxy + Auto Scaling EC2** — manages connection pooling (reduces DB connection overhead/timeouts), elastic compute for retries.
- (NOT ElastiCache for transactional writes — risk of data loss before persistence, doesn't fix connection saturation. NOT API Gateway throttling — suppresses demand rather than meeting it. NOT cross-Region read replicas — read-only, doesn't help write bottlenecks.)

---

## 12. Monitoring & Security Services

### 12.1 GuardDuty vs Inspector

| | Amazon GuardDuty | Amazon Inspector |
|---|---|---|
| Purpose | **Threat detection** — malicious activity monitoring | **Vulnerability assessment** |
| Scope | S3, accounts, network traffic (CloudTrail, VPC Flow Logs, DNS logs) + threat intel/ML | EC2 instances, container images, Lambda — scans for known vulnerabilities/exposure |

Never interchangeable — a question separating "detect malicious activity" from "scan for vulnerabilities" wants GuardDuty for the former, Inspector for the latter.

### 12.2 Near-Real-Time Automated Alerting on Abnormal API Activity

**Pipeline:** CloudTrail → CloudWatch Logs → **Metric Filter** → **CloudWatch Alarm** → **SNS** notification.
- CloudTrail's only native destinations: **S3 and CloudWatch Logs** (NEVER Kinesis directly — factual trap).
- Athena+S3+QuickSight = good for retrospective reporting/dashboards, NOT automated real-time alerting.
- Trusted Advisor + CloudWatch alarm = only fires on **service quota/limit breaches**, not abnormal usage patterns.

### 12.3 S3 Deletion Protection

- **Versioning** — "deletes" just add a delete marker; underlying data recoverable by removing the marker. Does NOT prevent permanent deletion of a specific version by version ID.
- **MFA Delete** — requires secondary auth for (1) permanently deleting an object version, (2) suspending versioning. Protects the versioning safety net itself.
- Both together = the standard "protect against accidental deletion" answer. (NOT: S3 console confirmation dialogs — don't exist. NOT: managerial approval process — not a technical control. NOT: SNS notification on delete — fires AFTER deletion, doesn't prevent it.)

---

## 13. Analytics / ML / NLP

### 13.1 Fully Serverless Analytics + SQL-based ML Pipeline

**"Serverless ETL + MPP warehouse + SQL-based ML, no Python":**
**Glue (serverless ETL) → Redshift Serverless (serverless MPP warehouse) → Redshift ML (SQL-based model training, uses SageMaker under the hood transparently)**.
(NOT RDS/Aurora — not MPP, built for OLTP not analytical aggregation at scale. NOT EMR/provisioned Redshift — not serverless, requires cluster management. NOT "Athena ML" — not a real production-fit service for this.)

### 13.2 Extracting Entities from Unstructured Text (no ML expertise)

**"Extract structured info from text, no in-house ML team, minimal overhead":** **S3 Event Notification → Lambda → Amazon Comprehend (custom entity recognition) → DynamoDB lookup**. Comprehend is fully managed NLP, no ML expertise needed.
(NOT SageMaker custom model — requires ML expertise to train/tune/maintain, over-engineered. NOT Lookout for Vision — that's for IMAGES, not text. NOT Transcribe round-trip — nonsensical/convoluted for a text-only input.)

---

## 14. Quick Decision Cheat-Sheet (exam trigger phrases)

| Phrase in question | Likely answer |
|---|---|
| "re-creatable data, infrequent access" | S3 One Zone-IA |
| "known/predictable access pattern, infrequent" | S3 Standard-IA (cheaper than Intelligent-Tiering) |
| "unknown/changing access pattern" | S3 Intelligent-Tiering |
| "exactly once + ordering" | SQS FIFO |
| "fully serverless, no capacity provisioning" | Lambda + SQS/Kinesis + DynamoDB (never EC2 anywhere in pipeline) |
| "maintain metric at X%" | Target Tracking scaling policy |
| "predictable/calendar-based scaling" | Scheduled Action |
| "HPC / tightly-coupled / low-latency node comm" | Cluster Placement Group |
| "fault-tolerant, interruptible, flexible" | Spot Instances |
| "dedicated + encrypted + low latency + high throughput" | Direct Connect + VPN |
| "on-prem AD + multi-account + centralized + low overhead" | AD Connector + IAM Identity Center |
| "cross-account, no internet/VPN/DX, specific service only" | AWS PrivateLink |
| "UDP / gaming / fast regional failover / keep own DNS" | AWS Global Accelerator |
| "dynamic website, on-prem backend stays, reduce latency immediately" | CloudFront with custom origin |
| "content-based routing" | Application Load Balancer (Layer 7) |
| "audit trail of key usage, no key management burden" | SSE-KMS |
| "S3 request rate/throttling fix" | Add more prefixes in same bucket |
| "keep using NFS, hybrid, automated tiering" | Storage Gateway File Gateway |
| "extreme parallel HPC + tied to S3" | FSx for Lustre |
| "SQL Server → Aurora PostgreSQL, minimal app change" | Babelfish + SCT/DMS |
| "OS-level DB customization + managed + minimal overhead" | RDS Custom |
| "minimize app refactoring, already on [DB type]" | Stay within same DB paradigm (don't switch SQL↔NoSQL) |
| "detect malicious activity" | GuardDuty |
| "scan for vulnerabilities" | Inspector |
| "strict on-prem data residency + modern AWS tooling" | AWS Outposts + EKS Anywhere |
| "text entity extraction, no ML expertise" | Amazon Comprehend |
| "geo-block by country, ALB-fronted app" | AWS WAF Geo Match |
| "geo-block by country, CloudFront/S3 distributed content" | CloudFront Geo Restriction |
| "in-memory + real-time + low latency (leaderboard, cache)" | ElastiCache (Redis) or DynamoDB+DAX |

---

*End of notes. Compiled from AWS Certified Solutions Architect – Associate practice questions and follow-up discussions in this session.*
