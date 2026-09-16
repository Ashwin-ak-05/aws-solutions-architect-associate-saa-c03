# AWS Solutions Architect Associate — Study Notes

Organized by topic. Each section explains **what the service/concept is**, **when to use it**, **when NOT to use it**, and the **decision rule** to spot it on the exam.

---

## 1. IAM & Access Control

### 1.1 IAM Policy Evaluation Logic
- AWS evaluates **all applicable policies** together. An explicit **Deny always wins** over an Allow, no matter where it comes from (identity policy, resource policy, SCP).
- A `Deny` with a `StringNotEquals` condition blocks an action **everywhere except** the named condition value.
- An `Allow` with a condition only grants access **when that condition is satisfied** — it does not override a Deny elsewhere unless the Deny's own condition excludes that case.
- **Trap pattern:** Deny all `service:*` unless `region = X`, then Allow a specific action with an IP condition. Net effect: that action only works inside region X **and** from the allowed IP — the Allow can never "escape" the Deny's region restriction.

### 1.2 `aws:SourceIP` condition key
- Refers to the IP address of **whoever is calling the AWS API** — never a property of the resource being created/managed (not the EC2 instance's own IP).
- Common confusion: people assume it restricts the **resource's** public/private/elastic IP. It does not — it's about **where the request originated**.

### 1.3 Service Control Policies (SCPs)
- Enforced at the **AWS Organizations** level, above IAM — sets the **maximum** permissions for every identity in a member account, **including the root user**.
- SCPs **cannot be bypassed from inside the account**, even by root — this is what makes them the right tool when a requirement says "prevent even root/admin from changing X."
- **SCPs do not affect the AWS Organizations management (master) account** — only member accounts.
- **Requires AWS Organizations to exist.** If the question doesn't mention Organizations, SCP is not a valid answer.

### 1.4 IAM Permission Boundaries
- A **managed policy** that sets the **maximum possible permissions** an IAM **user or role** can ever have, regardless of what identity-based policies get attached later (even by the user themselves).
- Effective permissions = **intersection** of identity-based policy AND permission boundary.
- **Can only be attached to IAM users or roles — never to IAM groups.**
- Use case: let developers self-attach AWS managed policies to experiment, but cap what they can ever actually do (prevents privilege escalation to `AdministratorAccess`).

### 1.5 SCP vs Permission Boundary vs regular IAM policy — decision table

| Requirement | Tool |
|---|---|
| Cap permissions for **every account** in an Org, even root | **SCP** (needs Organizations) |
| Cap permissions for **one IAM user/role**, even if they attach new policies to themselves | **Permission boundary** (user/role only, not groups) |
| A policy that just "denies an action" but user could remove it themselves | **Regular IAM policy** — weak, bypassable if the user can detach/modify it |

### 1.6 Cross-Account Resource Access (Lambda → S3 in another account)
- **Same account:** IAM policy on the caller's role is enough (ownership doesn't matter, only permissions).
- **Different accounts:** You need permission on **both sides**:
  1. The caller's IAM role/policy (Account A) must allow the action.
  2. The **resource-based policy** (e.g., S3 bucket policy) in Account B must also explicitly allow that specific role/account.
- This "both sides must agree" pattern applies broadly: S3 bucket policies, KMS key policies, SNS topic policies, Lambda resource policies, etc.

### 1.7 S3 Object Ownership Across Accounts
- **By default, an S3 object is owned by the AWS account that uploaded it — not the bucket owner.** This holds even if the object sits in someone else's bucket.
- Same-account access: controlled purely by IAM policy permissions; ownership is irrelevant.
- Cross-account access: the uploader's account owns the object by default, so the bucket owner does **not** automatically get access.
- **Fixes:**
  - **ACL `bucket-owner-full-control`** — set at upload time (e.g., `--acl bucket-owner-full-control`, or in Redshift's `UNLOAD ... ACL 'bucket-owner-full-control'`). Only works if the bucket still allows ACLs.
  - **Cross-account IAM role assumption** — uploader assumes a role trusted by the bucket owner; more robust, used by automated services.
  - **S3 Object Ownership = "Bucket owner enforced"** — modern, AWS-recommended default (since Apr 2023). Disables ACLs entirely; every object automatically becomes owned by the bucket owner regardless of uploader. No per-upload flags needed.

### 1.8 API Gateway Access Control by IP
- **API Gateway is a fully managed service — it does NOT run inside a VPC/subnet.** Therefore:
  - **Security groups do not apply to API Gateway** (SGs only apply to VPC-based resources: EC2, RDS, Lambda-in-VPC).
  - **Subnets/NACLs also don't apply.**
- Correct mechanism: a **Resource Policy** on the API — an IAM-style JSON document supporting `IpAddress` / `NotIpAddress` conditions to allow/deny by source IP.
- Even a **Private API** (reachable only via VPC endpoint/PrivateLink) doesn't mean API Gateway "runs in" your VPC — the endpoint is just a private doorway; the API Gateway service itself is still AWS-managed and outside your VPC.

---

## 2. Compute (EC2 / Auto Scaling)

### 2.1 EC2 Purchase Options — Tenancy/Isolation

| Option | Isolation level | Placement control | Cost | Use when |
|---|---|---|---|---|
| **Dedicated Instances** | Physical hardware isolated per AWS account | No visibility into physical host | Lower | Just need single-tenant hardware for compliance |
| **Dedicated Hosts** | Same isolation + full host visibility | Full control over core/socket/host placement | Higher | Need BYOL tied to physical cores/sockets, or specific instance placement control |
| **Spot Instances** | None (shared) | N/A | Cheapest | Fault-tolerant, interruptible workloads |
| **On-Demand** | None (shared) | N/A | Standard | No long-term commitment needed |

**Rule of thumb:** "single-tenant hardware for regulatory/compliance reasons" with no mention of licensing/placement control → **Dedicated Instances** (cheaper, sufficient). Only pick Dedicated Hosts if BYOL/per-core licensing or host placement visibility is explicitly needed.

### 2.2 Auto Scaling Group — Default Termination Policy (in strict priority order)
1. **Allocation strategy** for On-Demand vs Spot balance (if applicable).
2. **Instance with the oldest launch template or launch configuration** — but instances using a **launch configuration** are evaluated before ones using a launch template (i.e., if any instance uses a launch config, template-based instances are skipped first).
3. Among instances using **launch configurations**, terminate the one with the **oldest** launch configuration.
4. **Closest to the next billing hour** — used only as a final tiebreaker.
- Evaluation **stops at the first criterion that produces a clear winner** — later criteria are irrelevant once an earlier one decides it.

### 2.3 Why ASG Might NOT Terminate an Unhealthy Instance
- **Health check grace period hasn't expired** — ASG waits before acting on EC2/ELB health checks after instance launch.
- **Instance is `Impaired`** — ASG waits a few minutes for possible recovery instead of terminating immediately; may also delay if CloudWatch status-check data is incomplete.
- **ELB health check failed, but ASG's health check type is set to `EC2`** — ASG ignores ELB-reported failures unless health check type is explicitly set to `ELB`.
- **Myths (these do NOT block termination):**
  - Spot instances CAN be terminated by ASG normally.
  - Increasing the minimum instance count does NOT preserve unhealthy instances — ASG launches new healthy ones instead.
  - Custom health checks DO trigger termination (via `SetInstanceHealth`).

### 2.4 EC2 Hibernate — Speeding Up "Warm" Restarts
- **Problem it solves:** an application takes a long time to bootstrap/initialize every time an instance is stopped and started.
- **Hibernate** saves the **in-memory (RAM) state** to the EBS root volume on stop. On start, RAM is reloaded and processes **resume exactly where they left off** — no re-initialization needed.
- Contrast with:
  - **User Data** — just automates running startup commands; doesn't make them faster.
  - **EC2 Metadata** — just informational data about the instance; unrelated to startup speed.
  - **Custom AMI** — pre-bakes software/config for consistency across new instances, but a fresh instance from an AMI still has to fully boot and run its startup sequence from cold.

### 2.5 Achieving High Availability at Minimum Cost (Multi-AZ instance sizing)
- Rule: **spread instances across enough AZs so that losing any ONE AZ still leaves at least the required minimum instance count.**
- Formula: `(number of AZs − 1) × (instances per AZ) ≥ required minimum`, minimizing total instance count.
- Example: need ≥4 always available → 3 AZs × 2 instances = 6 total. If one AZ fails: 2 AZs × 2 = 4 remaining (exactly meets requirement, no waste).
  - 2 AZs × 2 each (4 total) fails — losing one AZ leaves only 2.
  - 2 AZs × 4 each (8 total) works but wastes cost (over-provisioned).

### 2.6 RDS/Aurora Multi-AZ — Engine Version Upgrade Causes Downtime
- Multi-AZ protects against **infrastructure/hardware failures** (automatic failover, minimal downtime) — but this does **not** extend to planned **database engine version upgrades**.
- During an engine upgrade, **both primary and standby are upgraded simultaneously** (they must stay in sync) — meaning there's **no healthy standby to fail over to**, so downtime is unavoidable during the upgrade window.
- **Trap:** don't assume "Multi-AZ = zero downtime always." It does not cover this specific planned-maintenance scenario.

---

## 3. Storage

### 3.1 S3 vs EBS vs EFS — Core Differences

| | Type | Access pattern | Concurrent multi-instance access? |
|---|---|---|---|
| **S3** | Object storage | API calls (GET/PUT), not a real file system | Not mountable as a filesystem natively |
| **EBS** | Block storage | Attached to a single EC2 instance | No (generally single-instance) |
| **EFS** | File storage (NFS) | Mountable, POSIX file system | **Yes — hundreds/thousands of instances concurrently** |

**Rule of thumb:** "many EC2 instances need to concurrently access the same shared files" → always **EFS**, never S3 or EBS.

### 3.2 EFS Performance Modes vs Throughput Modes (two SEPARATE settings — common trap)

**Performance Mode** (latency vs throughput tradeoff):
- **General Purpose** (default) — low latency; web servers, CMS, home directories.
- **Max I/O** — high aggregate throughput/IOPS across many parallel clients, at cost of slightly higher per-operation latency. Best for **highly parallelized workloads**: big data analytics, media processing, genomics.

**Throughput Mode** (bandwidth):
- **Bursting Throughput** — scales automatically with file system size.
- **Provisioned Throughput** — manually set fixed MiB/s, independent of storage size.

**Rule of thumb:** "big data / parallel / many concurrent clients" → **Max I/O** (performance mode). Throughput mode options (Bursting/Provisioned) are usually distractors in performance-mode questions.

### 3.3 S3 Encryption Options — Who manages the key vs who does the encryption

| Option | Who manages the key | Who performs encryption |
|---|---|---|
| **SSE-S3** | AWS (S3) | AWS (S3) |
| **SSE-KMS** | AWS (KMS) | AWS (S3) |
| **SSE-C** | **You** (bring your own key with every request) | AWS (S3) |
| **Client-Side Encryption** | You | **You** |

**Rule of thumb:** "customer must manage/store their own key (e.g., on-premises)" **but** "S3 should still do the actual encryption work" → **SSE-C**. If the customer also wants to do the encryption themselves → Client-Side Encryption.

### 3.4 AWS KMS Multi-Region Keys
- **You cannot convert an existing single-region KMS key into a multi-region key** — it's a hard limitation (preserves data residency guarantees).
- **You cannot "share" a KMS key across regions** either — keys are region-scoped by design.
- Multi-region keys are **created as multi-region from the start**; related keys in different regions share the same key material/ID, so you can encrypt in one region and decrypt in another without re-encryption or cross-region KMS calls.
- **To retrofit cross-region-same-key encryption onto existing S3 data:** create a **new bucket** with a **new multi-region KMS key**, **copy** existing data into it (re-encrypting under the new key), then enable **replication** to the target region.

### 3.5 Encrypting an Already-Existing (Unencrypted) RDS Database
- **RDS encryption can only be enabled at instance creation time — never afterward.** No "enable encryption" button exists for a running DB.
- **Workaround (the only valid path):**
  1. Take a **snapshot** of the unencrypted DB (snapshot = full copy of data + schema).
  2. **Copy** that snapshot with **encryption enabled**.
  3. **Restore** a new DB instance from the encrypted snapshot.
  4. Migrate the application to the new DB, then delete the old one.
- **Read replicas and Multi-AZ standby instances inherit the source's encryption status** — you cannot selectively encrypt just a replica/standby while the primary stays unencrypted.
- What encryption at rest actually does: encrypts the **entire underlying storage volume** (data + schema + indexes + backups) using AES-256 via a KMS key. Decryption happens **transparently** on authorized queries — no app changes needed. Protects against unauthorized access to **raw disk/storage**, not against normal authenticated access.

### 3.6 AWS Snowball → Long-Term Archival (cheapest path)
- **Snowball can only target an S3 bucket** — it **cannot** write directly to S3 Glacier or Glacier Deep Archive.
- **Cheapest approach:** Snowball job → S3 bucket → a **zero-day (same-day) lifecycle policy** transitions objects immediately to **Glacier Deep Archive**.
- Zero-day lifecycle policy means you're billed at the **destination tier's rate** from the start — you skip paying S3 Standard rates for any transit period.
- **Glacier Deep Archive is cheaper than regular Glacier** — use Deep Archive whenever the goal is maximum cost savings for long-term archival and fast retrieval isn't required.

### 3.7 AWS DataSync — On-Premises to EFS/S3 Migration
- Purpose-built for automating/accelerating large data transfers between on-prem storage and AWS storage services (S3, EFS, FSx family).
- **Direct Connect Virtual Interfaces (VIFs):**
  - **Public VIF** — reaches **public** AWS service endpoints (e.g., S3).
  - **Private VIF** — reaches **private** VPC resources (e.g., an interface VPC endpoint).
- **To sync on-prem NFS data directly into EFS with least operational overhead:** DataSync agent on-prem → **Private VIF** → **PrivateLink interface VPC endpoint for EFS** → scheduled DataSync task writes directly into EFS.
- **Common wrong patterns:**
  - Routing through **S3 + Lambda** as an intermediate hop when the destination is EFS — adds unnecessary complexity when DataSync can write to EFS directly.
  - **VPC Gateway Endpoint for S3** — only works for **VPC-internal** traffic; cannot be used to carry data over Direct Connect from on-premises.
  - **"VPC peering endpoint for EFS"** — not a real, valid construct for connecting on-prem to AWS over Direct Connect.

### 3.8 Amazon Macie
- Fully managed **data security/privacy service** using ML + pattern matching to **discover and classify sensitive data in S3** (PII, financial data, credentials).
- Flags risks like: sensitive data in a publicly accessible or unencrypted bucket.
- **Trigger phrase:** "automatically discover/classify sensitive data in S3" → **Macie**.

### 3.9 S3 Access Points
- Named network endpoints that simplify managing access to a **shared S3 bucket used by multiple applications/teams**.
- Instead of one large, complex bucket policy, create **separate access points**, each with its own scoped policy (and optionally restricted to a specific VPC).
- **Knowing an access point's ARN/hostname does NOT grant access** — access is still governed by IAM + the access point's own policy + the bucket policy (which must explicitly permit access via access points using `s3:DataAccessPointArn`).
- Access points are **created manually** (console/CLI/IaC) — not auto-generated.
- **Trigger phrase:** "many apps/teams need different access levels to the same shared bucket" → **S3 Access Points**.

---

## 4. Databases

### 4.1 Aurora Replicas vs RDS Multi-AZ Standby (common trap — different concepts!)
- **Aurora Replicas** — live, **readable** instances sharing the same cluster storage volume. Used to **offload read traffic** from the primary. Access via the **reader endpoint**, which load-balances across all available replicas. Up to **15 replicas** allowed (a ceiling, not a default — **0 replicas exist by default** unless you explicitly add them, or 1 if Multi-AZ is enabled at creation for failover purposes).
- **RDS (non-Aurora) Multi-AZ standby** — a **passive** failover target only. **Cannot be read from** during normal operation. This is an RDS-only concept; **Aurora has no "standby instance"** in this sense — Aurora's Multi-AZ setup uses Aurora Replicas instead, which ARE readable.
- **Fix for "reads are causing high I/O and slowing writes" on Aurora:** create/add an **Aurora Replica**, point read traffic to the **reader endpoint**.
- Aurora has **no built-in read-through caching** — would require adding ElastiCache separately (app code changes needed).

### 4.2 RDS for SQL Server — Migration with Least Operational Burden
- When migrating a sensitive, regulated on-prem SQL Server database to AWS with a goal of **minimizing management overhead**: use **Amazon RDS for SQL Server**, **Multi-AZ** (availability), and **KMS encryption at rest** (compliance) — a fully managed relational service.
- **Avoid:** EC2 + self-managed SQL Server (you're back to managing OS patching, backups, HA yourself); exporting to S3/CSV (loses relational functionality entirely); Amazon Timestream (built for time-series data, not general relational workloads — no joins/stored procedures/triggers support).

---

## 5. Messaging & Streaming

### 5.1 SQS vs SNS vs Kinesis vs Firehose — When to use which

| Service | Model | Best for |
|---|---|---|
| **SQS** | Pull-based queue; message deleted after processing | Decoupling producer/consumer; each message processed **once**, with retry/DLQ support |
| **SNS** | Push-based pub/sub fan-out | Broadcasting one event to **many subscribers** instantly; **cannot be polled** |
| **Kinesis Data Streams** | Ordered, replayable stream, partitioned by shards | Multiple independent consumers reading the **same** data repeatedly/at their own pace; real-time analytics with replay needs |
| **Firehose** | Buffers and batches, then delivers to a fixed destination (S3, Redshift, OpenSearch, Splunk, HTTP endpoint) | Streaming ingestion **into storage/analytics destinations**, not per-event application processing |

- **SNS cannot be polled** — it pushes to subscribers. An option saying "application polls the SNS topic" is always wrong.
- **Firehose does buffer, but only for batching delivery to a destination** — it is NOT a decoupling queue. Once delivered to S3, there's no per-record retry if your downstream app fails processing a specific file — you'd need to build that yourself. Firehose also cannot deliver directly to EC2; Lambda can only be used as an inline **transformation step**, not a final destination.
- **Direct S3 → Lambda triggers** can hit **Lambda's concurrency limits** (default ~1,000) during traffic bursts, risking throttled/dropped events. **Adding SQS as a buffer** between S3 and Lambda absorbs bursts — messages wait in the queue instead of being dropped, and Lambda polls at a sustainable rate.

**Rule of thumb:** "data loss during traffic spikes" + "no replay mechanism" + "minimize ops overhead" + "each event needs individual processing" → **S3 event → SQS → Lambda**. If it's really "stream data into a data lake/warehouse for analysis" → **Firehose**.

### 5.2 SQS FIFO with Message Group ID vs Kafka/Kinesis Partitions
- **Requirement pattern:** ordered processing **per independent entity** (e.g., per device), with ability to **scale consumers up toward the number of entities**.
- **Kafka partitions / Kinesis shards** — a **fixed, provisioned** number of parallel "lanes." Max consumer parallelism = number of partitions/shards, decided upfront and requiring manual resharding/increase requests to scale. In practice, this ceiling is far lower than the number of producers/entities (e.g., devices).
- **SQS FIFO + `MessageGroupId`** — no provisioned bucket/partition count. Every distinct Group ID becomes its own logical ordered lane; only one consumer processes a given group at a time, but the number of groups (and thus achievable parallelism) scales with **how many distinct Group IDs exist and how many consumers you run** — no infrastructure ceiling to plan around.
- **SQS FIFO with no Group ID at all** → entire queue treated as one strict sequence → only **1 consumer** possible, no scaling.
- **Standard SQS queue** → no ordering guarantees at all — fails any "must process in order" requirement.

**Rule of thumb:** "ordered per key" + "scale consumers toward number of keys/entities" → **SQS FIFO + Group ID**, not Kinesis (shard ceiling) and not FIFO without Group ID (no parallelism).

---

## 6. Serverless

### 6.1 Lambda Memory & CPU
- Memory is configurable **128 MB – 10,240 MB**, in 1 MB increments.
- **CPU scales automatically with memory** — you don't set CPU directly. At **1,769 MB**, you get the equivalent of **1 full vCPU**; scaling higher can grant up to **6 vCPUs**.
- If a Lambda function is slow due to **CPU-bound** work (not I/O-bound), increasing memory can speed it up (and sometimes even reduce total cost, since the function finishes faster despite a higher per-second rate).
- Ephemeral storage (`/tmp`) is a **separate** setting (512 MB – 10,240 MB). Timeout is also separate (max 15 minutes).

---

## 7. Analytics / ML / Security Ops

### 7.1 Textract + Comprehend — Document Text + Sentiment Analysis (least operational overhead)
- **Textract** — extracts text from complex/scanned PDFs (OCR), handling varied layouts/fonts.
- **Comprehend** — AWS's fully managed **NLP** service (Natural Language Processing) — pre-trained models for sentiment analysis, entity recognition, key phrases, topic detection. No training required.
- **Avoid:** SageMaker (requires building/training a custom model — heavy operational lift); Athena (just a SQL query engine — no NLP capability); Rekognition (computer vision for images/video, **not** text).
- **Rule of thumb:** "extract text from documents" + "analyze sentiment/tone/entities" + "least operational overhead" → **Textract + Comprehend**.

### 7.2 NLP (Natural Language Processing) — quick definition
- The branch of AI focused on understanding/interpreting human language (text/speech) — sentiment, entities, topics, language detection, key phrases. Amazon Comprehend is AWS's managed NLP API.

### 7.3 Amazon Security Lake
- Fully managed, **purpose-built** service to automatically **collect, normalize, and centralize** security-related data across AWS accounts/regions/services and third-party sources.
- Natively integrates with **CloudTrail, GuardDuty, VPC Flow Logs, AWS Config** — no custom ETL/ingestion pipelines needed.
- Normalizes everything into **OCSF** (Open Cybersecurity Schema Framework) — consistent, analyzable schema across different log sources.
- Stores data in an S3 bucket it manages (partitioning, retention, access management handled for you).
- **Avoid:** Lake Formation + Glue (general-purpose data lake tool for business data, not security-log-aware — requires custom ETL); Athena + QuickSight (analysis/visualization only, doesn't solve centralized collection); custom Lambda + CSV (heavy custom development).
- **Rule of thumb:** "centralize security event data across accounts" + "least development effort" → **Amazon Security Lake**.

### 7.4 AWS Config
- Tracks **configuration state and change history** of AWS resources; evaluates them against **rules** (managed or custom) to determine compliance (`COMPLIANT` / `NON_COMPLIANT`).
- Can stream findings/changes to an **SNS topic** for notifications.
- **AWS-managed rules** are pre-built and require no custom scripting — favor these whenever a question emphasizes "least scripting/maintenance."
- **Example use case:** monitor **imported (third-party) ACM certificates** for upcoming expiration (ACM auto-renews its own issued certs, but NOT imported ones) → use the managed Config rule for ACM certificate expiration, output to SNS. This beats manually building a CloudWatch alarm + custom action, which is more setup effort even though technically possible.

---

## 8. Networking & Content Delivery

### 8.1 Route 53 Routing Policies — pick by requirement

| Routing Policy | Purpose |
|---|---|
| **Latency-based** | Route to the region giving the user the **lowest measured latency** — use for **performance** problems |
| **Geolocation** | Route based on the **user's geographic location** — use for **compliance/content licensing restrictions**, not performance |
| **Failover** | Route to a backup resource only when the primary is **unhealthy** — use for **disaster recovery**, not load distribution |
| **Weighted** | Split traffic by percentage across resources — useful for A/B testing/blue-green, BUT subject to DNS caching delays |

**Rule of thumb:** "high latency / slow load time for users in region X" → **Latency-based routing** (+ regional read replicas/replica databases nearer those users).

### 8.2 AWS Global Accelerator — When DNS-based routing isn't good enough
- Provides **static anycast IP addresses** as a fixed entry point, directing traffic through **AWS's private global network/edge locations** to the nearest healthy endpoint (ALB, NLB, EC2, Elastic IP).
- **Does NOT rely on DNS at all** — traffic shifts (weights/dials) take effect **within seconds**, unaffected by client-side DNS caching (a major issue with mobile devices).
- **Supports TCP/UDP** — works with **NLBs** (Layer 4) directly, unlike CloudFront/ALB/WAF (Layer 7-only tools).
- **Use when:** (a) users are on mobile/DNS-caching-prone clients and you need **fast, predictable, global** traffic shifts (e.g., blue/green testing under a tight deadline); or (b) you have an existing **NLB-based** global architecture and need to **reduce latency without re-architecting** — Global Accelerator lets you register existing NLBs as endpoints with zero infrastructure changes.
- **Why alternatives fail in these scenarios:**
  - **Route 53 weighted/latency routing** — still DNS-based; subject to caching delays, especially on mobile.
  - **CloudFront** — Layer 7 (HTTP/HTTPS) only; **cannot front an NLB** (protocol mismatch — NLB is TCP/UDP).
  - **ALB + cross-zone load balancing** — ALB is also Layer 7 (wrong for non-HTTP/real-time traffic); cross-zone LB only balances **within** a region, doesn't address **global**, cross-region latency.

### 8.3 AWS Transfer Family — Legacy SFTP Vendors Uploading to S3
- Fully managed **SFTP-compatible endpoint** that stores uploaded files directly into S3 — vendors keep using their existing SFTP clients unchanged; no server management required.
- Combine with **per-vendor IAM roles** (scoped to specific bucket/prefix) for least-privilege access isolation between vendors.
- Add **identity federation** (Amazon Cognito, SAML, or custom IdP) to centrally manage and dynamically map many vendor identities to IAM roles — scales without manual per-user SFTP account management.
- **Avoid:** self-hosted EC2 + OpenSSH (violates "fully managed, no infrastructure" requirement); Route 53 private hosted zones (irrelevant — internal DNS only, no bearing on SFTP auth); Amazon AppFlow (SaaS-to-SaaS API integration tool — no SFTP support at all).
- **Rule of thumb:** "vendors stuck on legacy SFTP" + "fully managed, no infrastructure" + "map access per vendor to S3" → **AWS Transfer Family + IAM roles + identity federation**.

### 8.4 Sharing Private Network Access Across Many AWS Accounts (cheapest option)
- Scenario: multiple accounts (same region, same AWS Organization) need their EC2 instances to communicate privately.
- **Cheapest solution: create ONE VPC in a central account, and share its subnets with other accounts using AWS Resource Access Manager (RAM).** RAM is **free**.
- Other accounts launch EC2 instances **directly into the shared subnets** — since everything is effectively in the **same VPC**, private communication works natively with **zero extra networking components** (no peering, no gateways, no per-connection cost). Each account still owns/manages its own instances; only the networking resource (subnet) is shared.
- **Avoid:**
  - **AWS PrivateLink** — built for privately exposing a **specific service** to consumers, not general "all instances talk to each other" connectivity. Wrong use case, plus incurs per-endpoint costs.
  - **VPC Peering (full mesh)** — works, but doesn't scale well: requires a **separate peering connection + route table updates for every pair** of VPCs as account count grows.
  - **AWS Transit Gateway** — solves the same problem well at scale, but has **real ongoing costs** (hourly per-attachment + per-GB data processing charges) — more expensive than RAM-shared subnets for a same-region, same-Org scenario.

### 8.5 Centralizing Shared Services Across a Transit-Gateway Hub-and-Spoke Design
- Scenario: many VPCs (across accounts) already connected via **Transit Gateway** (hub-and-spoke); need to share common services (e.g., VPC endpoints, directory services) without duplicating them in every VPC.
- **Solution: build a single "shared services VPC"** hosting the common resources once; all spoke VPCs, already connected via the Transit Gateway hub, can reach it — reducing both **cost** (no duplication) and **admin overhead** (one place to manage/update).
- **Avoid:** Transit VPC (older, pre-Transit-Gateway pattern requiring self-managed EC2-based VPN appliances — extra ops burden + double data transfer charges); Fully meshed VPC Peering (connection count explodes as VPCs grow, hard to maintain, 125-peering-per-VPC cap); AWS Direct Connect (for connecting to **on-premises** data centers, not for linking VPCs to each other — also slow to provision, physical cabling).

### 8.6 API Gateway — see Section 1.8 above (IP-based access control).

---

## 9. Architecture Patterns

### 9.1 Warm Standby DR for Least Downtime Failover
- Scenario: on-premises data center is unreliable; need an AWS failover environment with **least possible downtime**, and data must stay uniform between on-prem and AWS.
- **Correct pattern (Warm Standby):** Route 53 **failover routing** + EC2 instances **already running** behind an ALB in an Auto Scaling group (not spun up on demand) + **AWS Storage Gateway (stored volumes)** continuously syncing data to S3.
- **Why "already running" matters:** any option relying on **CloudFormation to provision infrastructure at failover time** (even via Lambda automation) introduces provisioning delay — directly violating "least downtime." A pre-running standby avoids this entirely.
- DR strategy spectrum (recovery time vs cost): Backup & Restore (slowest/cheapest) → Pilot Light → **Warm Standby** (this pattern) → Multi-site Active/Active (fastest/most expensive).

### 9.2 WAF + ALB + Auto Scaling — Secure, Scalable, Highly Available Web Tier
- Standard "gold standard" pattern: EC2 in an **Auto Scaling Group across 2+ AZs**, fronted by an **ALB**, with **AWS WAF attached to the ALB**.
- **Critical fact: AWS WAF only integrates with Layer 7 entry points — ALB, CloudFront, API Gateway, App Runner, Global Accelerator.**
- **WAF CANNOT attach to:** a Network Load Balancer (NLB — Layer 4, protocol mismatch) or directly to an Auto Scaling Group (WAF isn't a compute-level construct).
- **Rule of thumb:** whenever a question pairs "WAF" with "NLB" or "attach WAF to the ASG directly," that combination is invalid — disqualify it immediately.

### 9.3 Two-Tier Security Group Design (Web + DB)
- Web tier (public subnets, port 443) → Security Group A: allow inbound 443 from `0.0.0.0/0` (public internet).
- DB tier (private subnets, port 1433/etc.) → Security Group B: allow inbound **only from Security Group A as the source** (not from a CIDR range) — meaning only the web tier can reach the database.
- **Why reference a security group instead of an IP range:** automatically covers any current/future instance in the source security group (e.g., after Auto Scaling adds/removes instances) without needing to update firewall rules manually.

### 9.4 Amazon Cognito — User Pools vs Identity Pools (frequent trap)

| | Purpose |
|---|---|
| **User Pools** | Manages **user identities and authentication** (sign-up/sign-in, MFA, tokens, federation with Google/Facebook/SAML) |
| **Identity Pools** | Provides **temporary AWS credentials** for accessing AWS resources — for authenticated AND unauthenticated (guest) users |

- **ALB has native integration with Cognito User Pools** — configure an "authenticate" action on a listener rule; **no custom code needed**.
- **CloudFront has NO native Cognito integration** — would require building a custom **Lambda@Edge** function, which is significant extra development effort.
- **Rule of thumb:** "decouple user authentication from application logic, minimal dev effort" + ALB in the architecture → **Cognito User Pools + ALB**. Identity Pools are never the answer for "authenticating users" — they're only for handing out AWS credentials.

### 9.5 S3 IAM Policy — Bucket-Level vs Object-Level Actions (common syntax trap)

| Action type | Example actions | Correct ARN pattern |
|---|---|---|
| **Bucket-level** | `s3:ListBucket`, `s3:GetBucketPolicy`, `s3:GetBucketLocation` | `arn:aws:s3:::mybucket` (no trailing `/*`) |
| **Object-level** | `s3:GetObject`, `s3:PutObject`, `s3:DeleteObject` | `arn:aws:s3:::mybucket/*` (with `/*`) |

- A correct **read-only** policy needs **two separate statements**: one granting `ListBucket` on the bucket ARN, and one granting `GetObject` on the bucket ARN + `/*`.
- Using the same (wrong-level) ARN for both actions, or swapping which action gets which ARN, silently breaks the policy (e.g., grants listing but no actual object reads, or vice versa).

---

## Quick-Reference "Trigger Phrase → Service" Cheat Sheet

| Exam phrase / scenario | Likely correct service/approach |
|---|---|
| "Concurrent access by many EC2 instances to shared files" | Amazon EFS (not S3, not EBS) |
| "Big data / highly parallel workload" on EFS | Max I/O performance mode |
| "Least development/config effort" + "centralize security logs across accounts" | Amazon Security Lake |
| "Extract text from PDFs" + "analyze sentiment" + "least ops overhead" | Textract + Comprehend |
| "Decouple auth from app logic" + ALB present | Cognito User Pools + ALB |
| "High latency for users in a specific region" | Route 53 latency-based routing + regional read replicas |
| "Mobile users + DNS caching + fast controlled traffic shift" | AWS Global Accelerator |
| "NLB + reduce global latency + keep infra unchanged" | AWS Global Accelerator (not CloudFront, not ALB) |
| "WAF" mentioned with "NLB" or "Auto Scaling Group directly" | Invalid combo — WAF only works with ALB/CloudFront/API GW/App Runner/Global Accelerator |
| "Legacy vendors only use SFTP" + "fully managed, no infra" | AWS Transfer Family + IAM roles + identity federation |
| "Many AWS accounts, same region/org, need private EC2-to-EC2 comms, cheapest" | Shared VPC subnets via AWS RAM |
| "Many VPCs via Transit Gateway need shared common services, reduce cost/admin" | Central Shared Services VPC |
| "Reads slowing down writes" on Aurora | Add Aurora Replica + use reader endpoint (not Multi-AZ standby — that's an RDS-only concept) |
| "Encrypt an existing unencrypted RDS DB" | Snapshot → copy as encrypted → restore new instance (only path; can't enable in place) |
| "S3 data must be encrypted/decrypted with same key across 2 regions" | New bucket + AWS KMS multi-region key + copy data + enable replication |
| "Customer must supply/manage their own key, but let S3 do the encrypting" | SSE-C |
| "Snowball to Glacier for cheapest long-term archive" | Snowball → S3 bucket → zero-day lifecycle policy → Glacier Deep Archive |
| "On-prem NFS data needs to land directly in EFS via Direct Connect" | DataSync + Private VIF + PrivateLink interface endpoint for EFS |
| "IP allow-list for API Gateway" | Resource Policy with IpAddress/NotIpAddress (never security groups — API GW isn't in a VPC) |
| "Prevent even root user from changing something" | Service Control Policy (needs AWS Organizations) |
| "Cap what one IAM user/role can ever do, even if they self-attach policies" | Permission Boundary (user/role only, not groups) |
| "Ordered per-entity processing, scale consumers toward entity count" | SQS FIFO + MessageGroupId (not Kinesis — shard count is a hard ceiling) |
| "Prevent data loss during traffic bursts, minimize ops overhead, per-event processing" | S3 event → SQS → Lambda (not direct S3→Lambda; not Firehose; not SNS — can't be polled) |
| "Discover/classify sensitive data automatically in S3" | Amazon Macie |
| "Multiple apps/teams need different access to same shared bucket" | S3 Access Points |
| "Automate a slow-starting app's resume time after stop/start" | EC2 Hibernate (not AMI, User Data, or Metadata) |
| "Least downtime DR failover, data stays uniform" | Warm standby: pre-running EC2/ALB/ASG + Storage Gateway (not CloudFormation-provisioned-on-demand) |
