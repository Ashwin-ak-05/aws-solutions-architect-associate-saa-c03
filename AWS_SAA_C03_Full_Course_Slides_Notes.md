# AWS Certified Solutions Architect – Associate (SAA-C03)

## Full Course Slides — Study Notes (v5.0)

**By Chetan Agrawal** ([www.awswithchetan.com](http://www.awswithchetan.com))  
Udemy Instructor | Ex. Senior Solutions Architect at AWS

*All rights reserved © www.awswithchetan.com — for personal exam-preparation use.*

---

## Table of Contents

- [Getting Started with AWS](#getting-started-with-aws)
- [AWS Identity and Access Management (IAM)](#aws-identity-and-access-management-iam)
- [Amazon EC2](#amazon-ec2)
- [EC2 Advanced](#ec2-advanced)
- [Elastic Load Balancer and Autoscaling Group](#elastic-load-balancer-and-autoscaling-group)
- [VPC and Networking](#vpc-and-networking)
- [Amazon S3](#amazon-s3)
- [AWS Container Services](#aws-container-services)
- [AWS Databases](#aws-databases)
- [Bigdata and Analytics](#bigdata-and-analytics)
- [Machine Learning and AI](#machine-learning-and-ai)
- [AWS Edge Networking](#aws-edge-networking)
- [Route 53 (DNS)](#route-53-dns)
- [AWS Serverless (API Gateway and Lambda)](#aws-serverless-api-gateway-and-lambda)
- [AWS Integration Services](#aws-integration-services)
- [AWS Data Security Services](#aws-data-security-services)
- [Infrastructure as Code (AWS CloudFormation)](#infrastructure-as-code-aws-cloudformation)
- [Application Deployment](#application-deployment)
- [AWS Monitoring and Logging](#aws-monitoring-and-logging)
- [AWS Systems Manager and More](#aws-systems-manager-and-more)
- [AWS Security Services](#aws-security-services)
- [Storage and Data Migration](#storage-and-data-migration)
- [Migration and Disaster Recovery](#migration-and-disaster-recovery)
- [AWS Multi-Account Management](#aws-multi-account-management)
- [AWS Billing and Cost Management](#aws-billing-and-cost-management)

---


## AWS Certified Solutions Architect
*(Slide 1)*

```text
                             Associate (SAA-C03)
                                                        Full course slides (v5.0)

                                              By Chetan Agrawal (www.awswithchetan.com)

                                              Udemy Instructor | Ex. Senior Solutions Architect at AWS



                                                                                                    Published Date: 05/07/2026
```


## Disclaimer and Copyrights
*(Slide 2)*

- These slides are copyrighted and intended strictly for personal use.
- This material is exclusively for learners enrolled in the “AWS Certified Solutions Architect – Associate (SAA-C03)” course by Chetan Agrawal
- Redistribution, sharing, or resale of this content is not permitted.
- These slides are provided solely for learning and exam preparation purposes.
- If you obtained this material from any source other than the official course platform, please report it to awswithchetan@gmail.com


## Course Sections
*(Slide 3)*

- Route 53 (DNS)
- Getting started with AWS
- AWS Serverless (API gateway and Lambda)
- AWS Identity and Access Management (IAM)
- Amazon EC2
- AWS Integration Services
- EC2 Advanced
- AWS Data Security Services
- Elastic Load Balancer and Autoscaling Group
- Infrastructure as Code (AWS CloudFormation)
- VPC and Networking
- Application Deployment
- Amazon S3
- AWS Monitoring and Logging
- AWS Container Services
- AWS Systems Manager and more
- AWS Databases
- AWS Security Services
- Bigdata and Analytics
- Storage and Data Migration
- Machine Learning and AI
- Migration and Disaster Recovery
- AWS Edge Networking
- AWS Multi-Account Management
- AWS Billing and Cost Management


# Getting Started with AWS

*(Source: Slide 4)*

### Getting started with AWS..


## Getting started with AWS..
*(Slide 5)*

1. AWS Global Infrastructure
2. AWS Account
3. Overview of AWS Services (from Solutions Architect perspective)


## Web
*(Slide 6)*

```text
                                                        Users                                 Browser
                                                                                                                              CloudFront
                       myapp.com on AWS
                                                         Route53            myapp.com                                                             Edge
                                                                                                                                                Locations


                         Region
                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                     Lambda

                                                Web                                                                             Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                               Convert
                                                                    S           S                Rekognition             S3                S3
                            SNS
                                                                                                           AI models                                    QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker


                            SQS
                                              ElastiCache

                                                                               Neptune        Kinesis                    S3              EMR
                                                                                                                                                            Redshift
                       CloudWatch                  Multi-AZ



                                                              RDS       DynamoDB              Glue
```


## AWS Global Infrastructure
*(Slide 7)*

1. Regions and Availability Zones
2. Edge Locations
3. Local Zones
4. Wavelength Zone
5. Outpost


## To deploy and run any application..
*(Slide 8)*

We need:
- Compute
- Storage                                myapp AWS Region
- Network
AWS Region AWS Region
Data Center


## AWS Regions
*(Slide 9)*

```text
                                                                         #1 – Low latency for end users

                                                                         #2 – Data Residency & Compliance

                                                                         #3 – Disaster Recovery

                                                                         #4 – AWS services pricing difference

                                                                         #5 – Availability of AWS services




                                              Refer https://aws.amazon.com/about-aws/global-infrastructure for latest
```


## AWS Region naming
*(Slide 10)*

- Every AWS Region has a name and a code
  - N. Virginia = us-east-1
  - Mumbai = ap-south-1
  - London = eu-west-2


## 1 Region = Multiple Availability Zones
*(Slide 11)*

```text
                                                        AZ        AZ

                                                             AZ




                                                https://aws.amazon.com/about-aws/global-infrastructure/
```


## 1 Region = Multiple AZs
*(Slide 12)*

```text
       1 AZ = Multiple data centers                                                                              Region


                                                  AZ1                                         AZ2
      Different floodplains                                        Up to 100 km
         (in most cases)

                                                                                              Data
                                               Data
        Redundant Power                       Center
                                                        Data
                                                       Center
                                                                                     Data
                                                                                    Center
                                                                                             Center    Data
                                                                                                      Center
            Supply                                              Low latency Fibre



     Redundant Network                                               AZ3
        Connectivity


                                                                  Data     Data
                                                                 Center   Center
                                                                                                               Region
```


## Why 3 or more Availability Zones in a Region?
*(Slide 13)*

```text
   High Availability and failover                          Region
                                                AZ1                     AZ2




                                              Primary DB




                                                            AZ3




                                                           DB Replica
                                                                              Region
```


## Why 3 or more Availability Zones in a Region?
*(Slide 14)*

```text
   High Availability and failover                                  Region
                                                        AZ1                 AZ2
   Durability
                                              upload               copy

                                                       S3 Bucket




                                                                    AZ3




                                                                                  Region
```


## Why 3 or more Availability Zones in a Region?
*(Slide 15)*

```text
   High Availability and failover                   Region
                                              AZ1            AZ2
   Durability

   Scaling and load distribution




                                                     AZ3




                                                                   Region
```


## Availability Zones
*(Slide 16)*

- There are minimum 3 AZs in each region         Mumbai Region AWS Region
- Availability Zone has name (or code) and IDs Availability Zone 1   Availability Zone 2   Availability Zone 3
- Same AZ name may map to different AZ ID for       (asp1-az1)            (asp1-az2)            (asp1-az3)
different AWS account
ap-south-1a
Customer #1               ap-south-1b
ap-south-1c
ap-south-1a Customer #2               ap-south-1b
ap-south-1c


## Beyond the AWS Region..
*(Slide 17)*

```text
      Access AWS Services…
                                                   Edge Locations




                                  AZ                                             Local
                                                                                 Zone
                               Region

                        AZ                    AZ                                                 Outpost



                                                                    Wavelength
                                                                      Zone
                                                                                         Local
                                                                                         Zone
```


## Getting started with AWS..
*(Slide 18)*

1. AWS Global Infrastructure
2. AWS Account
3. Overview of AWS Services (from Solutions Architect perspective)


## AWS Account
*(Slide 19)*

```text
                                                                                                       Route 53
                                                          AWS Account
                                                                                                       IAM



                                                                                                       VPC
                  Region 1 (N. Virginia, US)        Region 2 (London, UK)   Region 3 (Mumbai, India)
                                                                                                       S3 Bucket



                                                                                                        EC2
                   AZ1            AZ2         AZ3   AZ1      AZ2     AZ3    AZ1      AZ2       AZ3

                                                                                                        RDS
                  AZ4             AZ5         AZ6
```


# AWS Identity and Access Management (IAM)

*(Source: Slide 20)*

### AWS IAM

*Identity & Access Management*


## AWS core services
*(Slide 21)*

```text
                                                          IAM
                                                        Security




                                                                    Networking
                                              Compute




                                                          Storage
                                              EC2         S3        VPC


                                                                                 AWS
```


## AWS                             Root User
*(Slide 22)*

```text
                   Account
  IAM

                                                                                     Developer
                                                     SRE/DevOps
                         Network
                          admin




                                                                           ML Developer



                                                        IAM policy
                                                                     Data
                                                                     Scientist


                                              DBA
```


## In this section
*(Slide 23)*

- AWS account and Users
- Accessing AWS
- IAM Credentials – Username/password, Access Key, Temporary Credentials
- IAM Policy, types and features
- IAM Group
- IAM Role
- AWS CLI, SDK and AWS CloudShell
- IAM Permissions boundary
- IAM Tools – IAM Access Analyzer, IAM Policy Simulator & Policy generator
- IAM Audit and compliance
- IAM Best practices


## AWS Account and Users
*(Slide 24)*


## AWS Account and users
*(Slide 25)*

```text
                     AWS account                                                                                      Global service
                                                                        Users
                                                           Root User                        AWS Identity and Access       Free
                                                                                              Management (IAM)
                                                           IAM User




                                        Region 1                       Region 2        Region X ..



                                              Amazon EC2                  Amazon EC2       Amazon EC2

                                              Amazon RDS                 Amazon RDS        Amazon RDS

                                       More services                   More services    More services
```


## AWS Account - User types
*(Slide 26)*

```text
                                              Unrestricted access
                    Root User                   to AWS account




                                              Restricted access to
                                                 AWS account
              IAM User / Role
```


## Root user, IAM user & group and IAM Role
*(Slide 27)*

```text
                                Identities


                                              Unrestricted permissions
                              Root User


                                                Restricted permissions
      Developer
       Group                                                             AWS Account
                               IAM User
     IAM Group



                               IAM Role
                                                        IAM Policy
                                                      (one or more)
```


## AWS account user types
*(Slide 28)*

Root User                               IAM User / Role
- Owner of the AWS account
- Individual users or services or applications having specific permissions
- IAM policy is not applicable and hence no
- IAM policy is applicable and can grant control over the permissions                            granular permissions
- Use only when you need to perform special
- Should be used to by an individual person or actions (account closure, billing access, payment       application with only required access to AWS etc.)                                                   services and resources to perform the job.
- Not recommended for day-to-day operations
- Recommended for day-to-day operations


## Tasks that require Root User Access
*(Slide 29)*

```text
      ✓ Change your account Settings
      ✓ Restore IAM User Permission
      ✓ Close your AWS Account
      ✓ Activate IAM access to the Billing and Cost Management Console
      ✓ Configure an Amazon S3 bucket to enable MFA
      ✓ Change AWS support plan
```


## Accessing AWS
*(Slide 30)*


## Accessing AWS..
*(Slide 31)*

AWS = Amazon Web Service
REST APIs (HTTPS based)
Directly invoking low level APIs is difficult EC2
- SigV4 signing
- Request formatting
- Response parsing (JSON/XML) User                                                                           S3
VPC


## Accessing AWS..
*(Slide 32)*

```text
                                                        AWS = Amazon Web Service

                                                               REST APIs
                                                             (HTTPS based)



                                     AWS Management
                                        Console                               EC2




     User                                     AWS CLI                         S3




                                              AWS SDK                         VPC
```


## Accessing AWS..
*(Slide 33)*

```text
                                                              Amazon Web Service

                                                                   REST APIs
                                                        IAM      (HTTPS based)



                                     AWS Management
                                        Console                                    EC2




     User                                     AWS CLI                              S3




                                              AWS SDK                              VPC
```


## IAM credentials
*(Slide 34)*


## IAM Credentials
*(Slide 35)*

```text
                                                                  IAM

                  AuthN = Username + password


                                              AWS Management
                                                                                EC2
                                                 Console       Authentication
                    AuthN = IAM Access Key
                                                                     +
                                                               Authorization
     User                                        AWS CLI                        S3


                    AuthN = IAM Access Key
                                                                 IAM policy
                                                 AWS SDK                        VPC
```


## IAM Password policy
*(Slide 36)*

- There is a default Password policy which enforces certain restrictions on passwords such as – minimum length 8 chars, mix of char types etc.
- You can define custom password policy to define:
  - Password minimum length
  - Password strength (e.g. at least one uppercase, lowercase, special char etc.)
  - Turn on password expiration (1 to 1095 days)
  - Allow or deny users to change their own password
  - Prevent password reuse (1 to 24 previous passwords)


## Demo - IAM password policy
*(Slide 37)*


## Exercise : Create an IAM user
*(Slide 38)*

```text
            1       Login to AWS Management Console using Root user credentials (email/password) and navigate to IAM service

            2       Click account id (Top right) and Turn on multi-session support

            3        Go to Users -> Create user -> Provide username (say Ben) -> Provide user access to AWS
                     Management console -> I want to create IAM user -> Custom password -> Enter the password ->
                     Uncheck users must create a new password at next sign in -> Next

            4       On the permissions screen -> Do not attach any policies at this moment

            5       Go to IAM Dashboard page and copy the account sign-in URL under AWS account details

            6       This time provide IAM username/password to login to AWS

            7       Once logged in, you can check whether you can perform all AWS actions. Can you access EC2, S3 or IAM ?


                At this moment, your AWS account should have 3 users – Root User, admin IAM user (created as a
                                           part of pre-requisites) and Ben IAM user
```


## IAM Credentials
*(Slide 39)*

```text
                                                                   IAM

                  AuthN = Username + password
                                        +MFA
                                               AWS Management
                                                                                 EC2
                                                  Console       Authentication
                   AuthN = IAM Access Keys
                                                                      +
                                                                Authorization
     User                                         AWS CLI                        S3


                    AuthN = IAM Access Key


                                                  AWS SDK         IAM policy     VPC
```


## Multi-factor authentication
*(Slide 40)*

- Multi-Factor Authentication adds a second layer of security
- Once enabled, user will need to provide not only a password to authenticate but also temporary digital token sent through a preset device like a smartphone running an Authenticator app.
+   Password   +                    = Successful login Email ID or Username                                        Multi-factor           to AWS Account (Root user or IAM user)                                    Authentication (optional)


## Multi-factor Authentication (MFA)
*(Slide 41)*

```text
          Virtual Authentication                    FIDO security keys                   Hardware TOTP tokens
                   Apps




          Google Authenticator                By third-party providers such        By Thales              By Hypersecu
         Microsoft Authenticator              as Yubico, acs, Gotrust etc.    (3rd party provider)     (3rd party provider)
              More apps..
```


## Assignment : Set up MFA for IAM User
*(Slide 42)*

1        Download an AWS compatible Authenticator App e.g. Google Authenticator
2        Login to AWS Management Console using Root user and navigate to IAM.
3        Click on your IAM Username for which you want to setup MFA
- Click Users on the left navigation panel and click on the user for which you want to set up MFA
- Click on Security credentials tab
- Click on Manage in Assigned MFA device row
- Select Virtual MFA device and click Continue
- You will see a dialog window with instruction to setup MFA and a Show QR code button
- Click on Show QR in the above dialog so that you can scan it using your app
4        Open Authenticator App installed in Step 1 and scan QR code.
- App detects your account.
- Click on Add ACCOUNT in Authenticator app to add your AWS account in the authenticator app.
- Enter 2 Consecutive MFA codes from your Authenticator App.
5        Verify MFA Setup
- Log out of your account and try to login again.
- You will be prompted for an MFA code after you enter your username/password, provide MFA from your authenticator App.


## IAM credentials
*(Slide 43)*


## IAM Credentials
*(Slide 44)*

```text
                                                                  IAM

                  AuthN = Username + password


                                              AWS Management
                                                                                EC2
                                                 Console       Authentication
                    AuthN = IAM Access Key
                                                                     +
                                                               Authorization
     User                                        AWS CLI                        S3


                   AuthN = IAM Access Key
                                                                 IAM policy
                                                 AWS SDK                        VPC
```


## IAM credentials – Access key
*(Slide 45)*

- Access keys are long-term credentials for an IAM user or the IAM user AWS account root user.
- Used to sign AWS API programmatic requests directly or via AWS CLI or AWS SDK (SigV4)
- Access keys consist of two parts:                                             Access Key
  - Access key ID =~ Identifies user
  - Secret access key =~ Like password
- A user can have maximum 2 active Access keys at any time Access key ID          Secret access key
- It can not be re-generated.
- After generating Access key, store both Access key id and secret access key securely.                                     AKIAIOSFODNN7EXAMPLE
wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY


## Exercise : Create IAM user access key
*(Slide 46)*

```text
            1       Login to AWS account using Root user or IAM admin user


            2       Go to IAM console -> Users -> Select the user for which IAM access key to be created


            3       Go to Security Credentials -> Access Keys -> Create access key -> Save the keys locally
```


## IAM policy
*(Slide 47)*


## IAM Policy
*(Slide 48)*

- In AWS, a policy is a JSON document that defines permissions to access AWS services and resources.
- These policies are associated with AWS IAM users, groups, and roles to control their permissions


## IAM Policy
*(Slide 49)*

```text
                                                                              Amazon Web Service

                                                                                          REST APIs
                                                                        IAM             (HTTPS based)
                                              600+ actions just for                 RunInstances
                                                 EC2 service                        DescribeInstances
                                                                                    StartInstances
                                                                                                        EC2

                                                                                      CreateBucket
                                                                                      ListBucket

                                                                                      GetObject
 IAM User                                                             IAM policy                        S3
                                                                                      PutObject
 IAM Role



                                                                                                        VPC
```


## IAM Policy
*(Slide 50)*

```text
                                                    Amazon Web Service

                                                               REST APIs
                                              IAM            (HTTPS based)


                                                       IAM Policy

                                                                             EC2




 IAM User                                                                    S3
 IAM Role



                                                                             VPC
```


## IAM Policy components
*(Slide 51)*

```text
                                                                   IAM Policy

                                              Principal   Effect      Action     Resource    Condition




                                                                      Which
                                                          Allow                               When
                                                                    Services &    Which
                                               Who?        or                                Condition
                                                                     Actions     Resources               EC2
                 User                                     Deny                               matches
                                                                                                         EC2
```


## IAM Policy
*(Slide 52)*

- IAM Policy is a JSON document
What is JSON?
Example IAM Policy: Start or stop instances based on tags


## JSON
*(Slide 53)*

- JSON (JavaScript Object Notation) is a lightweight, text-based format used to store and exchange data.
- Uses Simple key-value pairs
  - "Action": "s3:ListBucket“
- Supports nesting (multi-level) structure
- Readable by both Humans and the Computers


## IAM Policy
*(Slide 54)*

- IAM Policy is a JSON document
- Policy Version: 2012-10-17
- Id: string-to-identify-policy (optional)
- Statements: one or more statements
- Each statement has:
  - Sid: Identifier for the statement (optional)
  - Effect: Allow or Deny access
  - Principal: AWS account or IAM user or IAM role to which this policy is applicable
  - Action: List of actions (use * for all)
  - Resource: Particular resources for which permissions are granted (use * for all)
  - Condition: Apply policy statement only when condition is valid (optional)
Example IAM Policy: Start or stop instances based on tags


## Sample IAM policies
*(Slide 55)*

```text
           {
                       User can start/stop EC2 instances              Users not allowed to Terminate instances
               "Version": "2012-10-17",
               "Statement": [
                                                                       {
                   {                                                      "Version": "2012-10-17",
                     "Effect": "Allow",                                   "Statement": [
                     "Action": [                                            {
                       "ec2:StartInstances",                                   "Effect": "Deny",
                       "ec2:StopInstances",                                    "Action":
                       "ec2:DescribeInstances"                          "ec2:TerminateInstances",
                                                                               "Resource": "*"
                     ],
                                                                            }
                     "Resource": "arn:aws:ec2:ap-south-1:123456789012:instance/*"
                                                                          ]
                   }                                                    }
               ]
           }
```


## Sample IAM policies
*(Slide 56)*

```text
           {
                   To launch EC2 instances only in Mumbai region         Lambda to upload/download S3 objects
               "Version": "2012-10-17",
               "Statement": [
                                                                     {
                   {                                                   "Version": "2012-10-17",
                       "Effect": "Allow",                              "Statement": [
                       "Action": "ec2:RunInstances",                     {
                       "Resource": "*",                                    "Effect": "Allow",
                       "Condition": {                                      "Action": [
                                                                             "s3:GetObject",
                           "StringEquals": {
                                                                             "s3:PutObject"
                               "aws:RequestedRegion": "ap-south-1"
                                                                           ],
                           }
                                                                           "Resource": "arn:aws:s3:::my-app-
                       }                                             bucket/*"
                   }                                                     }
               ]                                                       ]
           }                                                         }
```


## IAM policy types
*(Slide 57)*


## IAM Policy types
*(Slide 58)*

- Identity based policy -> Attached to IAM user, group or role
- Resource based policy -> Attached to AWS resource e.g. S3 bucket
IAM User                          IAM Policy   S3   IAM Policy


## Identity based policy
*(Slide 59)*

```text
                                                                            Re-usable
                                                                                                         AWS
          What permissions IAM entity has?                                                          Managed Policies
                                                                    Managed Policies

                                                                                                   Customer Managed
                                                                                                        Policies


                IAM User
               IAM Group                      IAM Policy               In-line policy
               IAM Role
                                                      embedded directly inside a specific IAM user, group, or
                                                                    role. Can’t be re-used.
```


## Identity based IAM policy - Example
*(Slide 60)*

```text
           {                                                          Users not allowed to Terminate instances
                   User can start/stop EC2 instances
               "Version": "2012-10-17",
               "Statement": [                                            {
                   {                                                        "Version": "2012-10-17",
                       "Effect": "Allow",                                   "Statement": [
                                                                              {
                       "Action": [
                                                                                 "Effect": "Deny",
                         "ec2:StartInstances",
                                                                                 "Action":
                         "ec2:StopInstances",                             "ec2:TerminateInstances",
                         "ec2:DescribeInstances"                                 "Resource": "*"
                       ],                                                     }
                                                                            ]
                       "Resource": "arn:aws:ec2:ap-south-1:123456789012:instance/*"
                                                                          }
                   }
               ]
           }
```


## Resource based policy
*(Slide 61)*

- A resource-based policy is attached                   Who has permissions to access directly to a resource instead of being                      this resource? attached to an IAM user, group, or role. Account
- It defines who (principal) can access the                     S3 bucket resource and under what conditions.
- It includes a "Principal" element — which identity-based policies do not
- Supported by S3, KMS, SQS, SNS and many other AWS services
Resource based policy


## Resource based policy – Use case
*(Slide 62)*

```text
                                              Cross-account Access to S3 bucket

       Account A                                               Account B
                                                                                  S3 bucket




                        Identity based policy                              Resource based policy
```


## Resource based policy - Examples
*(Slide 63)*

```text
                Make S3 bucket publicly accessible                   Allows an external AWS account
                 (e.g. For hosting static website)       Account A to send messages to your Queue in Account B.

   {                                                 {
     "Version": "2012-10-17",                          "Version": "2012-10-17",
     "Statement": [                                    "Id": "CrossAccountSQSAccess",
       {                                               "Statement": [
         "Sid": "PublicRead",                            {
         "Effect": "Allow",                                "Sid": "AllowAccountAtoSend",
         "Principal": "*",                                 "Effect": "Allow",
         "Action": "s3:GetObject",                         "Principal": {
         "Resource": "arn:aws:s3:::my-public-                 "AWS": "arn:aws:iam::111122223333:root"
   bucket/*"                                               },
       }                                                   "Action": "sqs:SendMessage",
     ]                                                     "Resource": "arn:aws:sqs:ap-south-
   }                                                 1:444455556666:MyQueue"
                                                         }
                                                       ]
                                                     }
```


## Exercise: Create IAM policies
*(Slide 64)*

```text
                                                         Customer Managed
                                                               Policy


                                              IAM User


                                                           Inline Policy
```


## Exercise : Create IAM policies
*(Slide 65)*

```text
            1        Login to AWS account using admin IAM user and Create a new IAM policy with the name say
                     “CustomPolicy-S3-Full-Access”. Use following JSON to define the policy permissions.

                     {
                         "Version": "2012-10-17",
                         "Statement": [
                           {
                             "Sid": "Statement1",
                             "Effect": "Allow",
                             "Action": "s3:*",
                             "Resource": "*"
                           }
                         ]
                     }


             2      Attach this policy to IAM user that you created earlier (Ben in my case)

             3      Also create and attach Inline policy to this user. (See Policy document in the next slide.)
```


## Exercise : Create IAM policies
*(Slide 66)*

```text
            4        Inline policy to grant EC2 read access to this user.
                       {
                           "Version": "2012-10-17",
                           "Statement": [
                             {
                               "Effect": "Allow",
                               "Action": [
                                   "ec2:Describe*"
                               ],
                               "Resource": "*"
                             }
                           ]
                       }

            5       Go to S3 console and see if this user can create a new S3 bucket. Provide unique bucket name.

             6       Go to EC2 console and see if this user can view EC2 details. Can the user launch an ec2 instance?

                   Tip: Generally adding AWS account id in the bucket name is a good practice to make sure
                                               that bucket names are unique
```


## IAM policy - Conditions
*(Slide 67)*


## IAM Policy conditions
*(Slide 68)*

- Conditions in IAM policies let you fine-tune access control beyond just who and what. {
- They specify "when", "where", or "how" a policy is effective.       "Version": "2012-10-17",
- Condition Operators:                                                "Statement": [ {
  - StringEquals or StringNotEquals "Effect": "Allow",
  - Bool                                                          "Action": "ec2:RunInstances",
  - IpAddress / NotIpAddress                                      "Resource": "*", "Condition": {
  - DateGreaterThan / DateLessThan                                  "StringEquals": {
- Condition keys:                                                             "aws:RequestedRegion": "ap-south-1" }
  - aws:RequestedRegion }
  - aws:SourceIp                                                }
  - aws:MultiFactorAuthPresent                                ] }
  - aws:username
  - aws:CurrentTime


## Common policies with conditions
*(Slide 69)*

- Allow launching EC2 only in North Virginia region using condition key aws:RequestedRegion { "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": "ec2:RunInstances", "Resource": "*", "Condition": { "StringEquals": { "aws:RequestedRegion": “us-east-1" } } } ] }


## Common policies with conditions
*(Slide 70)*

- Enforce MFA for the sensitive actions using condition key aws:MultiFactorAuthPresent { "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": [ "s3:DeleteObject", "s3:PutBucketPolicy" ], "Resource": "arn:aws:s3:::my-sensitive-bucket/*", "Condition": { "Bool": { "aws:MultiFactorAuthPresent": "true" } } } ] }


## Common policies with conditions
*(Slide 71)*

- Deny all action if not logged in from the corporate network using condition key aws:SourceIp
{ "Version": "2012-10-17", "Statement": [ { "Sid": "DenyAllOutsideCorporateNetwork", "Effect": "Deny", "Action": "*", "Resource": "*", "Condition": { "NotIpAddress": { "aws:SourceIp": [ "203.55.22.0/24“ ] } } } ] }


## Common policies with conditions
*(Slide 72)*

- Allow each user to access only their own folder/path in S3 using condition key aws:username { "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": "s3:*", "Resource": [ "arn:aws:s3:::my-company-data/${aws:username}", "arn:aws:s3:::my-company-data/${aws:username}/*" ] } ] }


## Common policies with conditions
*(Slide 73)*

- Allow EC2 actions only during office hours (9 AM – 6 PM UTC) using condition key aws:CurrentTime { "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": [ "ec2:StartInstances", "ec2:StopInstances“ ], "Resource": "*", "Condition": { "DateGreaterThan": { "aws:CurrentTime": "2025-10-26T09:00:00Z" }, "DateLessThan": { "aws:CurrentTime": "2025-10-26T18:00:00Z" } } } ] }


## IAM Policy – Good to know
*(Slide 74)*


## Versioning IAM policies
*(Slide 75)*

- When a change is made to existing IAM policy, a new version of the policy is created
- This ensures that you can rollback the policy in case there is any side effect observed
- You can have up to 5 versions
Default


## IAM policy effect – Deny vs Allow
*(Slide 76)*

- IAM principal is denied access by default. This is called Implicit Deny.
- An implicit denial occurs when there is no applicable Deny statement but also no applicable Allow statement
- If policies only include Allow Statements, then the access is granted Explicit Deny
- If policies include an Allow statement and a Deny statement, the Deny statement trumps the Allow statement. This is called Explicit Deny.
Explicit Allow
Implicit Deny (default)
“Explicit Deny overrides Allow.”


## Policy 1                         Policy 2
*(Slide 77)*

```text
        {                                              {
             "Version": "2012-10-17",
                                                           "Version": "2012-10-17",
             "Statement": [
                                                           "Statement": [
                  {
                                                               {
                      "Sid": "AllowStart",
                                                                   "Sid": "DenyEC2",
                      "Effect": "Allow",
                      "Action":"ec2:StartInstances",               "Effect": "Deny",

                      "Resource": "*"                              "Action": "ec2:*",
                  }                                                "Resource": "*"
             ]                                                 }
        }                                                  ]
                                                       }
```


## IAM Group
*(Slide 78)*


## IAM Group
*(Slide 79)*

```text
                                     IAM User




                                                                         AWS Account
                                                                Access
                                 IAM Group

                                                  IAM Policy
                                                (one or more)
```


## IAM Groups
*(Slide 80)*

IAM Groups Developers Group
- Create IAM group and add IAM users to the group
- Instead of associating IAM policies to individual   Developers users, associate IAM policies to the group
- A group can contain multiple users, and user can                     Quality/Test belong to multiple groups.                                         Engineers Group
Quality/Test engineers
Site-Reliability Engineers
Operations engineers


## Assignment: IAM group
*(Slide 81)*

1      Login as admin IAM user and create a new IAM group say ‘DevOps’ and attach following AWS managed policies to this group
- AmazonCloudWatchFullAccess
- AmazonSSMFullAccess
- AWSEC2FullAccess
- AWSCloudFormationFullAccess
- AWSLambda_FullAccess
2      Login to AWS account as Ben (you can have multiple sessions in AWS console) and try creating a new Lambda function -> Access denied error 3      Now Add Ben user to this group
4      Try creating a new Lambda function
- Use a Blueprint -> Select Hello World function (Python)
- Function Name: MyFirstLambdaFunction
- Create function -> Should be successful


## AWS CLI
*(Slide 82)*

*AWS Command Line Interface*


## Accessing AWS
*(Slide 83)*

```text
                                                                  IAM

                  AuthN = Username + password


                                              AWS Management
                                                                                EC2
                                                 Console       Authentication
                    AuthN = IAM Access Key
                                                                     +
                                                               Authorization
     User                                        AWS CLI                        S3


                    AuthN = IAM Access Key
                                                                 IAM policy
                                                 AWS SDK                        VPC
```


## Accessing AWS using AWS CLI
*(Slide 84)*

```text
                                                                          IAM




                                                                       Authentication
                                              AuthN = IAM Access Key
                                                                             +
                                                                       Authorization



                                                                         IAM policy



                   AWS CLI
```


## AWS CLI
*(Slide 85)*

- A Command Line Interface (CLI) to access AWS
- Install CLI into your workstation (as per the operating system)
- Configure AWS CLI by providing:
  - AWS region -> default region for CLI. Provide region code.
  - Access Key ID -> your access key id
  - Secret access key -> your secret access key
  - Output format -> text or json
- Access AWS using CLI commands:
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html


## Exercise : Using AWS CLI
*(Slide 86)*

1        Install AWS CLI into your workstation (follow the steps as per your workstation operating system) https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
2        Configure AWS CLI with Ben user Access key credentials (you had created access key previously)
- AWS region -> Provide default region code.
- Access Key ID -> your access key id
- Secret access key -> your secret access key
- Output format -> text or json
3       Run the AWS CLI commands: aws ec2 describe-instances //should be successful with empty list aws s3 ls                  //should show buckets in your account


## AWS SDK
*(Slide 87)*

*AWS Software Development Kit*


## AWS SDK
*(Slide 88)*

- AWS web services are exposed through       Java      AWS SDK for Java v2 REST & HTTP APIs Node.JS   AWS SDK for Java v2
- SDK wraps these APIs to handle low level   Python    Boto3 operations e.g. SigV4 request signing, authentication, headers management          C++      AWS SDK for C++
C#      AWS SDK for .NET      AWS APIs
- SDKs are language specific Go      AWS SDK for Go v2
- Cloud based Applications are built using corresponding language SDK                  Ruby     AWS SDK for Ruby v3
Kotlin   AWS SDK for Kotlin
… Which SDK does AWS CLI use?                  SDK Python Boto3


## Assignment : Use SDK to Launch & terminate ec2 instance
*(Slide 89)*

```text
        Assuming you are using Ben user’s access key configured in your terminal for AWS CLI:


           1      In your terminal, install python3 and boto3
                  https://boto3.amazonaws.com/v1/documentation/api/latest/guide/quickstart.html


           2      Download the following scripts into your workstation:

               https://raw.githubusercontent.com/awswithchetan/aws-solutions-architect-associate/refs/heads/main/launch_ec2.py
               https://raw.githubusercontent.com/awswithchetan/aws-solutions-architect-associate/refs/heads/main/terminate_ec2.py

           4      Launch EC2 instance by running the script (provide region code parameter):
                  > python3 launch_ec2.py <region code>

           5      Go to AWS console for your region and check if you see EC2 instance running

           4     Terminate EC2 instance by running the script (provide region code and instance id parameters):
                 > python3 terminate_ec2.py <region code> <instance_id>
```


## AWS CloudShell
*(Slide 90)*


## AWS CloudShell
*(Slide 91)*

*CloudShell*


## IAM Role
*(Slide 92)*


## IAM User, Group and Role
*(Slide 93)*

```text
                                     IAM User




                                                                         AWS Account
                                                                Access
                                 IAM Group

                                                  IAM Policy
                                                (one or more)

                                  IAM Role
```


## Why IAM Role?                                    {
*(Slide 94)*

```text
                                                                                    IAM Policy
                                                           "Version": "2012-10-17",
                                        IAM user           "Statement": [
                 Long term                                   {
                 credentials                                   "Effect": "Allow",
                                                               "Action": [
                       Access key                                "s3:GetObject",
                                                                 "s3:PutObject"
                                                               ],
                                                               "Resource": "arn:aws:s3:::my-app-
                                                         bucket/*"
                                                             }
                                                           ]
                                                         }
                                      Long term
                                      credentials


                                                    Upload/Download file
                                              App

                               EC2
                                                                                           S3
```


## Why IAM Role?                                    {
*(Slide 95)*

IAM Policy "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": [ "s3:GetObject", "s3:PutObject" ], "Resource": "arn:aws:s3:::my-app-
- Valid for short duration           bucket/*"
- Rotated every 1 hr by                  } default ] } IAM Role For EC2
Upload/Download file App
EC2 S3


## IAM Role
*(Slide 96)*

- An IAM role is an AWS identity that can be assumed temporarily by users, applications, or services to securely access AWS resources without using long-term credentials.
- IAM Role is used by:
  - AWS service such as Amazon EC2, Lambda to access other AWS service or resource
  - An IAM user in the same or a different AWS account (e.g. for cross-account access)
  - An external user authenticated by an external identity provider (IdP) service such as Google, Facebook to get access to AWS account
IAM user
External user IAM Role     IAM Policy AWS                                                                                           AWS Resources Services


## IAM Role temporary credentials
*(Slide 97)*

- Credentials are valid for short duration Security Token Service (STS)       •   Rotated every 1 hr by default
Access key ID + Secret Access key + Session Token
AWS Account IAM Role
IAM Policy (one or more)


## Exercise: Create IAM Role for EC2
*(Slide 98)*

```text
                                                                                         Exercise steps

                We will revisit this exercise in EC2 section                1   Launch EC2 Linux instance in a default
                                                                                VPC and connect to it over SSH.


                                                                            2
                                                                                Create S3 bucket in the same region
                                                                                and upload some sample text or image
                                                                                file.

          Mumbai Region                                                         From EC2 terminal, try to download file
                                                                            3
                                                                                from S3 using AWS CLI command –
                                                                                Access denied.
                 VPC                                                            Go to AWS IAM and create IAM role for
                                   IAM Role                                 4
                                                                                EC2. Attach S3 full permissions policy to
                                                                                the role.

                                                                            5   Associate this new role to EC2 instance
                                              Upload/download
                                                                            6   Try again to download file from S3 –
                            EC2                                 S3 bucket       should be successful.

                                                                            7
                                                                                Terminate EC2 instance. Optionally delete
                                                                                S3 bucket.
```


## IAM Advanced Topics
*(Slide 99)*

- IAM Role types – Service Account, Service-linked, Cross-account, Federated role and AWS STS service
- Hands-on exercise for cross-account IAM role
- AWS Organization and more
- AWS IAM Identity Center (successor to AWS Single Sign-On)


## AWS APIs, CLI, SDK - Putting it together
*(Slide 100)*


## Another way to look at it..
*(Slide 101)*

User Applicat
- AWS services are exposed through REST (HTTP)           AWS                             ion CLI
s APIs                                                                     AWS SDK
- AWS SDK wraps these AWS APIs to handle low level                      User Applications PHP Python operations e.g. request signing, authentication,                               IAM headers management. SDKs are language specific. .NET
- Applications are typically built using corresponding          Java           AWS
IAM language SDK                                                                 APIs Java
IA
- AWS APIs are protected using IAM. User and                                          M        script Node AWS application needs IAM permissions to invoke AWS                                                        Console C# APIs                                                                    Go
ns C++ tio a Example:                                                               lic
  - AWS CLI uses python (boto3) SDK                             User App
  - AWS Console uses Java script SDK


## IAM Permissions Boundary
*(Slide 102)*


## Permissions boundary
*(Slide 103)*

- A permission boundary is an advanced IAM feature that sets the maximum permissions an IAM user or role can have — even if their policies allow more.
- Think of it as a “guardrail” that defines the upper limit of access.
- It does not grant the additional permissions
IAM User
IAM Role IAM Policies          Permission Boundary


## IAM Access Advisor
*(Slide 104)*

```text
                                         IAM Policy                 Permissions Boundary

         {                                            {
                 "Version":"2012-10-17",                  "Version":"2012-10-17",

                 "Statement": [                           "Statement": [
                     {                                        {
                         "Effect": "Allow",                       "Effect": "Allow",
                         "Action": [                              "Action": [
                             "s3:*",                                  "ec2:*",
                             "cloudwatch:*",                      ],
                             "ec2:*"                              "Resource": "*"
                         ],                                   }
                         "Resource": "*"                  ]
                     }                                }
                 ]
         }
```


## IAM Tools
*(Slide 105)*


## IAM Tools
*(Slide 106)*

- IAM Access Analyzer
- IAM Policy Simulator
- IAM Policy Generator


## IAM Access Analyzer
*(Slide 107)*


## IAM Access Analyzer
*(Slide 108)*

- A tool that continuously monitors and analyzes IAM policies (including resource-based policies like S3 buckets, KMS keys, and IAM user & roles policies)
- It tells you “who can access your resources”
- It helps detect unused access, unintended public or cross-account access.
- Helps maintain least privilege and compliance by detecting risky permissions.


## IAM Access Analyzer - Findings
*(Slide 109)*

- Resource Analysis – External Access
  - Scans resources which can be accessed by external identities (e.g. Public S3 buckets)
- Resource Analysis – Internal Access
  - Which all roles or users can access specific resource (e.g. KMS key)
- Unused Access
  - Unused roles – Roles with no access activity within the specified usage window.
  - Unused IAM user access keys and passwords – Credentials belonging to IAM users that have not been used to access your AWS account in the specified usage window.
  - Unused permissions - Permissions not used within specified usage window
For unused permission findings, IAM Access Analyzer can recommend policies to remove from an IAM user or role and provide new policies to replace existing permissions policies.


## IAM Policy Simulator
*(Slide 110)*


## IAM Policy simulator
*(Slide 111)*

- Used to test IAM policies before applying and troubleshoot AccessDenied issues
- Simulates if a specific action e.g. check if s3:PutObject is Allowed or Denied.
- Can test inline, managed, and resource-based policies.
- Available via AWS Console (or CLI): https://policysim.aws.amazon.com/


## IAM Policy Generator
*(Slide 112)*


## IAM Policy Generator
*(Slide 113)*

- There are two ways to automatically generate IAM policy
1. Create a new policy using graphical tool
2. Create a policy based on AWS account activities by the user or a role


## New policy generator
*(Slide 114)*

- Use this graphical tool: https://awspolicygen.s3.amazonaws.com/policygen.html
- Provides a web-based wizard to select Service → Actions → Resources.
- Generates valid JSON policies for users, groups, or roles.
- Supports multiple policy types – Identity based policies (IAM) and Resource based policies (S3, SNS, SQS)


## Generate IAM policy based on CloudTrail activity
*(Slide 115)*

- IAM Access Analyzer analyzes your CloudTrail events to identify actions and services that have been used by an IAM entity (user or role)
- How it works?
  - Go to IAM User or Role -> Permissions -> Generate Policy based on CloudTrail events
  - Set up a time range up to 90 days for IAM Access Analyzer to analyze historical CloudTrail events
  - Generate Policy
  - Review and customize policy
  - Attach policy to the user or a role


## IAM Audit & Reports
*(Slide 116)*


## AWS IAM Reports
*(Slide 117)*

- IAM Credentials Report
  - Generate and download a credential report that lists all users in your account and the status of their various credentials, including passwords, access keys, and MFA devices.
  - Used for Auditing and compliance
There are other IAM reports such as Organization activity, Service Control Policies (SCP) and Resource control policies (RCP) access reports which we will talk about in AWS Organization section


## IAM Best practices
*(Slide 118)*


## AWS IAM Best Practices
*(Slide 119)*

- Do not use Root user for day-to-day activities, instead use IAM user
- Apply strong password policy for console access
- Enable multi-factor authentication (MFA) for root user and IAM users
- Use temporary credentials with IAM roles to access AWS
- Rotate access keys regularly for use cases that require long-term credentials and keep them safe.
- Always apply Least-privilege permissions when creating or assigning IAM policies
- Use permissions boundaries to restrict and delegate permissions management within an account
- Use conditions in IAM policies to further restrict access e.g. Allow access when: "aws:ResourceTag/environment": “development“ or "aws:RequestedRegion": "ap-south-1“ etc.
- Detect public and cross-account access to resources with IAM Access Analyzer
- Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions
- Use IAM Access Analyzer to generate least-privilege policies based on CloudTrail access activities
- Regularly review and remove unused users, roles, permissions, policies, and credentials


## AWS Compute services
*(Slide 120)*

*EC2, ECS, EKS and Lambda*


## Physical server vs Virtual Machines vs Containers
*(Slide 121)*

```text
                                                       ✓   OS/Libraries flexibility                    ✓   Lightweight – shares underlying OS
      ✓     High Performance                                                                           ✓   Faster to deploy
      ✓     Physical isolation / License compliance    ✓   VM level resource utilization flexibility
                                                                                                       ✓   Portability
      X     Resource contention for Apps
      X     Noisy neighbor problem                             App                     App
                                                            Bin/Library            Bin/Library                App                  App
                                     Virtual Machine
                                                                 OS                     OS                 Bin/Library         Bin/Library
                                                                 VM               ne     VM                 Container            Container
              App                             App
                                                                      Hypervisor                             Container Runtime
                        Bin/Library

              Operating System                                 Operating System                               Operating System




                       Hardware                                       Hardware                                      Hardware
                   Physical Servers                                Virtual Machines                                  Containers
```


## Physical server vs Virtual Machines vs Containers
*(Slide 122)*

```text
  ✓      OS/Libraries flexibility                    ✓   Lightweight – shares underlying OS
  ✓      VM level resource utilization flexibility   ✓   Faster to deploy
                                                     ✓   Portability                                     Function code
                                                                                                          or Container
               App                            App
                                                                                                             image
          Bin/Library                 Bin/Library           App                  App
                OS                            OS         Bin/Library         Bin/Library
                 VM                  ne       VM          Container            Container


                      Hypervisor                           Container Runtime

              Operating System                              Operating System


                                                                                                    Output
                       Hardware                                   Hardware
                  Virtual Machines                                 Containers                 Serverless Compute
```


## Virtual Machines vs Containers vs Serverless
*(Slide 123)*

```text
  ✓      OS/Libraries flexibility                    ✓   Lightweight – shares underlying OS
  ✓      VM level resource utilization flexibility   ✓   Faster to deploy
                                                     ✓   Portability                                     Function code
                                                                                                          or Container
               App         IaaS               App
                                                                    PaaS                           FaaS      image
          Bin/Library                 Bin/Library           App                  App
                OS                            OS         Bin/Library         Bin/Library
                 VM                  ne       VM          Container            Container


                      Hypervisor                           Container Runtime
                                                       ECS - Elastic     EKS - Elastic              Lambda
              Operating
                    EC2 System                             Operating System
                                                                       Kubernetes Service
                                                     Container Service



                                                                                                    Output
                       Hardware                                   Hardware
                  Virtual Machines                                 Containers                 Serverless Compute
```


# Amazon EC2

*(Source: Slide 124)*

*Elastic Compute Cloud*


## Web
*(Slide 125)*

```text
                                                        Users                                 Browser
                                                                                                                              CloudFront
                       myapp.com on AWS
                                                         Route53            myapp.com                                                             Edge
                                                                                                                                                Locations



                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                     Lambda

                                                Web                                                                             Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                               Convert
                                                                    S           S                 Rekognition            S3                S3
                            SNS
                                                                                                           AI models                                    QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker


                            SQS
                                              ElastiCache

                                                                               Neptune        Kinesis                    S3              EMR                Redshift
                                                   Multi-AZ
                       CloudWatch


                                                              RDS       DynamoDB              Glue
```


## What is EC2?
*(Slide 126)*

EC2            EC2            EC2 Instance       Instance       Instance
- Elastic Compute Cloud                             App
- Virtual Machine in AWS Cloud                     VM 1            VM 2           VM N Customer
- AWS customers get access to EC2 instance
- Customer do not have access to underlying physical hardware                    Hypervisor (KEM, Xen, HyperV ..)
AWS
CPU             RAM           Disk Network               Physical Server


## Where EC2 is hosted?
*(Slide 127)*

```text
                                               Mumbai Region



      AWS Region -> Availability Zone -> EC2


                                                       EC2

                                                         AZ1         AZ2




                                                               AZ3
```


## Configuration options for EC2
*(Slide 128)*


## Configuration options for EC2
*(Slide 129)*

- Name of the instance (Optional) - Tag (key-value)
- Operating System - Amazon Machine Image (AMI)
- CPU, Processor family, Memory - Instance Type
- Login credentials - SSH key pair
- Network, IP address (Public / Private / Elastic), AZ - VPC
- Storage Disk - Elastic Block Storage (EBS)
- Firewall - Security Group
- Purchasing Option - OnDemand (default), Spot, Savings Plan, Reserved Instances
- Hardware Tenancy - Shared (default), Dedicated


## Other configuration options
*(Slide 130)*

- Initialization Scripts - User Data
- IAM Permissions - IAM Role


## Process to launch & connect to EC2 instance
*(Slide 131)*

```text
                                              Region
                                               AWS Region       Availability Zone 1              Availability Zone 2
          IAM User                               Public Key

                                                        VPC (default)

         Browser
            Private
        Private Key




                                                                 Security group
                                 SSH
                                                                                         EBS                           AMIs
                                                                                  EC2   volume
       SSH Client
                                                              EC2 Public IP (54.23.45.67))
```


## Ways to connect to EC2 instance
*(Slide 132)*

1. Connect over SSH using SSH Client (e.g. Terminal or PuTTY) – For Linux EC2
2. Connect over Remote Desktop using RDP client - For Windows EC2
3. EC2 Instance Connect over the browser – For Linux EC2
4. AWS Session Manager (SSM) over the browser – For Linux and Windows EC2
5. EC2 Instance connect endpoint


## SSH Key-pair
*(Slide 133)*

- A key pair consists of a public key and a private key
- For Linux instances, the private key allows you to securely SSH into your instance
- For Windows instances, the private key is required to decrypt the administrator password.
- SSH Key pairs are regional
- We can use same key-pair to launch more than one instances. All those instances will have same security credentials.


## EC2 Instance Connect
*(Slide 134)*

- Amazon EC2 Instance Connect provides a secure and convenient way to connect to your Linux instances over SSH
- It uses your IAM permissions to generate and use temporary SSH keys for login
Requirements
- The instance must have the EC2 Instance Connect package installed (Amazon Linux 2, 2023 and Ubuntu 20.04+ AMIs has it by default, for other OS install the instance connect package)
- SSH Port 22 must be open in the Security Group.
- The IAM user or role must have IAM permission: ec2-instance-connect:SendSSHPublicKey


## SSH vs EC2 Instance Connect
*(Slide 135)*

SSH                           EC2 Instance Connect
- Connect using SSH Client
- Connect over the browser session
- Uses permanent / Long term key pairs
- Generates temporary ssh keys and uses IAM
- Do not need IAM permissions                      permissions to push ssh keys to EC2 instance
- Needs manual key management
- Needs IAM permissions
- More secure as no key management
- Logs access in AWS CloudTrail
You can launch EC2 instance without SSH key pair. In that case, which of the above method will you use to connect to the instance?


## Exercise - Launch EC2 instance (Linux)
*(Slide 136)*

```text
                                              Region
                                               AWS Region       Availability Zone 1              Availability Zone 2
          IAM User                               Public Key

                                                        VPC (default)

         Browser
            Private
        Private Key




                                                                 Security group
                                 SSH
                                                                                         EBS                           AMIs
                                                                                  EC2   volume
       SSH Client
                                                              EC2 Public IP (54.23.45.67))
```


## Exercise : Launch EC2 instance (Linux)
*(Slide 137)*

```text
          1        Launch EC2 (Linux) instance in Mumbai Region
                   a) Go to EC2 Service -> EC2 Dashboard -> Launch Instances
                   b) Name: MyEC2Linux
                   c) Select Application and OS Images (Amazon Machine Image): Amazon Linux 2023 (default)
                   d) Select instance type: t2.micro (default)
                   e) Select key pair : Your key-pair that you had created earlier in pre-requisites
                   f) Network settings -> Default VPC
                   g) Make sure Auto-Assign Public IP is enabled
                   h) Firewall -> Create security group
                        a) Allow SSH traffic from -> Select My IP [Type-> SSH, port Range-> 22, source type -> My IP]
                        b) Allow HTTP traffic from -> Everywhere-IPv4
                   a) Configure Storage -> 8GiB, gp3 (default)
                   b) Launch Instance

          2       Connect over SSH using SSH client
                  a) If using windows workstation – Open Putty session, Load key file and use EC2 public IP or DNS for connection
                  b) If using Mac or Linux workstation – Open terminal and use ssh command: ssh -i <keyfile> <ec2 public IP>
                  c) Use login user as ec2-user
```


## Exercise : Launch EC2 instance (Linux)
*(Slide 138)*

```text
          3        Connect to EC2 instance using EC2 instance instance
                   a) Open the Amazon EC2 console​
                   b) In the navigation pane, choose Instances.
                   c) Select the instance and choose Connect.
                   d) Choose the EC2 Instance Connect tab.
                   e) Choose Connect using a Public IP.
                   f) Choose Connect to establish a connection. An in-browser terminal window opens.

          4       Terminate the instance
                  a) EC2 console -> Select your instance -> Instance State -> Terminate instance
```


## Assignment : Launch EC2 instance (Windows)
*(Slide 139)*

```text
                  Launch EC2 (Linux) instance in Mumbai Region
                  a) Go to EC2 Service -> EC2 Dashboard -> Launch Instances
                  b) Name: MyEC2Windows
                  c) Select Application and OS Images (Windows): Microsoft Windows Server 20XX Base
                  d) Select instance type: t2.micro (default)
                  e) Select key pair : Your key-pair that you had created earlier in pre-requisites
                  f) Network settings -> Default VPC
                  g) Make sure Auto-Assign Public IP is enabled
                  h) Firewall -> Create security group
                  i) Allow RDP traffic from -> Select My IP [Type-> RDP, port Range-> 3389, source type -> My IP]
                  j) Configure Storage -> 8GiB, gp3 (default)
                  k) Launch Instance
            2     Retrieve Windows Administrator password
                  a) Go to EC2 console -> Select your instance -> Actions -> Get Windows Password
                  b) Paste your .pem key file content (or browse and load .pem file). AWS will provide you the decrypted
                     Windows password
            3     Connect over RDP
                  a) Open RDP session using Remote Desktop Client to Public IP/DNS of EC2 Windows instance
                  b) Provide Username= Administrator, Password= <decrypted password>
            4     Terminate the instance
                  a) EC2 console -> Select your instance -> Instance State -> Terminate instance
```


## EC2 Instance Types
*(Slide 140)*

```text
                                                          EC2 Instance
                                                             Types



          General                     Compute       Memory         Storage    Accelerated     HPC
          Purpose                     Optimized    Optimized      Optimized   Computing     Optimized


                                         C7g C7a    R8g R7i         I4g I4i     P5 P4        Hpc7g
          M7g M7a
                                          C6i C5    R6a R5         D3 D2      G6 G4dn        Hpc7a
           M6i M5
                                        C5n C5a     U7i X2gd          H1       Trn1 Inf1     Hpc6a
           M4 T4g

            T3      T2                        C4    X1   Z1d                      F1
```


## EC2 Instance naming                                                            b – block storage optimization
*(Slide 141)*

```text
                                                                                       d – instance store volume
                                                                                       e – extra storage or memory
                                                   Instance         Additional         n – Network and EBS optimized
                                                  generation        capability         More..




                                               c7gn.xlarge
       C - Compute Optimized
       D - Dense storage
       F - FPGA
       G - Graphics Intensive
       I - Storage Optimized
       Hpc – HPC optimized
       P - GPU accelerated
       M - General Purpose                    Instance                                     Instance Size
                                                           Instance
       T - Burstable Performance               Family                                    medium      large       xlarge   2xlarge 4xlarge 16xlarge
                                                          processor
       U - High Memory
                                                                                  vCPU          1       2           4        8       16       64
       VT- Video Transcoding
       X - Memory intensive                          a - AMD Processor            Memory        2       4           8       16       32      128
       More..                                        g - AWS Graviton Processor
                                                     i - Intel Processor
```


## Choosing right EC2 instance type
*(Slide 142)*

- Identify workload type - Understand whether it’s general purpose, compute-heavy, memory-heavy, or storage-intensive.
- Resource requirements - Estimate vCPUs, memory, storage size, and network throughput your application needs.
- Pricing – Use AWS Pricing calculator to check and compare instance pricing
- Start small and scale as required and think of horizontal scaling
= m6.2xlarge       m6.xlarge      m6.xlarge


## Configuration options for EC2
*(Slide 143)*

- CPU, Processor family, Memory - Instance Type
- Firewall - Security Group
- Storage Disk - Elastic Block Storage (EBS)
- Operating System - Amazon Machine Image (AMI)
- Login credentials - SSH key pair
- Network, IP address (Public / Private / Elastic), AZ - VPC
- Initialization Scripts - User Data
- IAM Permissions - IAM Role
- Purchasing Option - OnDemand (default), Spot, Savings Plan, Reserved Instances
- Hardware Tenancy - Shared (default), Dedicated
- Name of the instance (Optional) - Tag (key-value)


## Security Group - Firewall for EC2 instance
*(Slide 144)*

- Security Groups
- Network Access Control List (NACL)
VPC
Subnet
N   Inbound Traffic Security   A EC2 Group     C L   Outbound Traffic


## Security Group
*(Slide 145)*

- Security Groups are most basic, native and important firewall for EC2 instances
- Security group has Inbound and Outbound rules
- Security group has only ALLOW rules. Does not support DENY/Block rules.
- Default Security group in each VPC
- Authorises traffic for both IPv4 and IPv6 traffic
- Security groups are stateful – return traffic is automatically allowed
ssh     Inbound Rules 22
http 80 Outbound Rules tcp xxxx
IP 5.6.7.8 Security Group EC2


## Typical ports – good to know
*(Slide 146)*

Inbound rules Protocol     Port        Source
- 22 = SSH (Secure Shell) for logging into a Linux    HTTP        80         0.0.0.0/0                       EC2 server instance                                      SSH        22          <My IP>
- 3389 = RDP (Remote Desktop Protocol) for                                                          http logging into a Windows server instance                                                             80 Inbound traffic
- 21 = FTP (File Transfer Protocol) for uploading                                               ssh 22              tcp files to a file share                                                                                          xx
- 22 = SFTP (Secure File Transfer Protocol) for                                                                    Outbound traffic
uploading files securely (over SSH)
- 80 = HTTP for accessing websites or web Outbound rules
applications                                                             Protocol          Port          Destination
All traffic        All           0.0.0.0/0
- 443 = HTTPS for TLS secured websites


## Security Groups - Summary
*(Slide 147)*

- Security group rules enable you to filter traffic based on protocols and port numbers. SG
- Single Security Group can be attached to multiple instances
- Single instance can have up to 16 Security groups with maximum 1000 rules*
- All inbound traffic is blocked by default
- By default, security groups contain outbound rules that allow all outbound traffic.
- Authorises traffic for both IPv4 and IPv6 traffic                               SG1
- Security groups are stateful - if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. SG2
- You can add and remove rules at any time. Your changes are automatically applied to the instances that are associated with the security group.
SG3 *limits may change in the future


## Storage for EC2 = EBS
*(Slide 148)*

- EBS is Elastic Block Storage service which provides Availability Zone persistent block storage for EC2 instance
- EBS is a Storage Attached Network (SAN) and hence volumes can be persisted irrespective of EC2 lifecycle         EC2
- We can create one or more EBS volumes (disk) and attach to EC2 instance
- EC2 has a root volumes (contains operating system) and optionally can have one or more data volumes
- EBS volumes are elastic – we can increase the size of EBS volumes as required                                                             Snapshots EBS volume
- EBS volume data can be backed up using Snapshots


## Instance store
*(Slide 149)*

- EC2 disks can be of 2 types:                                                  EC2
  - Elastic Block Storage (external to EC2 host) Root Volume
  - Instance Store (optional) – Local to EC2 host      EBS     n/w C:\     /dev/sda1
- EBS volume data is persisted until volume is deleted D:\        Data Volumes
- Instance Store volume data is wiped out if EC2 is   EBS /dev/sdh stopped and started.                                  EBS           E:\ /dev/sdf
EBS
Instance Store
Physical Host


## EBS vs Instance store
*(Slide 150)*

```text
                                  EC2                            EC2


                         Root Volume                EBS   C:\   Root Volume
                                              C:\


                       Data Volumes
                                              D:\   EBS   D:\   Data Volumes

                                      E:\

                                                    EBS


                          Instance Store                   Instance Store

                           Physical Host 1                  Physical Host 2
```


## Instance store
*(Slide 151)*

- EC2 disks can be of 2 types:                                                      EC2
  - Elastic Block Storage (external to EC2 host) Root Volume
  - Instance Store (optional) – Local to EC2 host          EBS           C:\     /dev/sda1 n/w
- EBS volume data is persisted until volume is deleted
- Instance Store volume data is wiped out if EC2 is D:\ stopped and started.                                    EBS                      Data Volumes /dev/sdh EBS           E:\
- EBS volumes can be created and attached to EC2                                   /dev/sdf instance. Instance store volume has size limit and comes with specific EC2 instance types only.            EBS
- EBS has per GB cost. Instance volumes are free.
- EBS is used for almost all purposes whereas Instance                        Instance Store store can be used for temp directory, buffer or cache Physical Host


## Configuration options for EC2
*(Slide 152)*

- CPU, Processor family, Memory - Instance Type
- Firewall - Security Group
- Storage Disk - Elastic Block Storage (EBS)
- Operating System - Amazon Machine Image (AMI)
- Login credentials - SSH key pair
- Network, IP address (Public / Private / Elastic), AZ - VPC
- Initialization Scripts - User Data
- IAM Permissions - IAM Role
- Purchasing Option - OnDemand (default), Spot, Savings Plan, Reserved Instances
- Hardware Tenancy - Shared (default), Dedicated
- Name of the instance (Optional) - Tag (key-value)


## Amazon Machine Image (AMI)
*(Slide 153)*

- An AMI provides information required to launch an instance.
- You can launch multiple instances from a single AMI when you require multiple instances with the same configuration.
- An AMI includes the below:                                                           AMI
  - A template for the root volume for the instance
  - One or more Amazon EBS snapshots                         Account
- Launch permissions that control which AWS accounts can use                /dev/sda1
AMI to launch instances                                             EC2   /dev/sdh
- You can use AMIs from AWS Community or AWS Marketplace volumes AMIs or you can create your own AMIs.
- AMIs are region specific


## Amazon Machine Image (AMI)
*(Slide 154)*


## Creating custom AMI
*(Slide 155)*

```text
                                                              Add data volume(s)

          Operating system and
               packages                                         Install packages,
                                                              software, Applications



                                              Launch               Create AMI
                                                       EC2

                          AMI                                                            AMI
                         (Base)                                                        (custom)

                                                        EC2
                                                       EC2
```


## AMIs are regional
*(Slide 156)*

```text
                                                          Copy AMI




                                   Region A                    Region B


                                                    AMI                         AMI

                                              EC2                         EC2
```


## Virtual Private Cloud (VPC)
*(Slide 157)*

```text
      A virtual private cloud (VPC) is a virtual network dedicated to your AWS account. It is logically isolated from
      other virtual networks in the AWS Cloud.


                                Physical Network                                     AWS VPC

                                 Router

                                                                              Subnet A            Subnet B




                                                                               EC2                 EC2
                                                                                         Router

                              Hub or Switch   Hub or Switch



                                                                               AZ1                  AZ2
```


## Default VPC                                                                           Internet
*(Slide 158)*

- AWS Creates Default VPC in each AWS         Region                                                   172.31.0.0/16 region Availability Zone        Availability Zone        Availability Zone
- VPC with CIDR 172.31.0.0/16
- If you don’t select any VPC then this                                               Internet Gateway default VPC is used to launch an EC2 instance                                         VPC 172.31.1.0/24          172.31.2.0/24
- Create one subnet in each AZ                            172.31.0.0/24 Subnet                   Subnet                   Subnet
- Has components like Internet Gateway, route table to allow communication over the internet
- EC2 instance receives the Private IP from the subnet CIDR range and Public IP from the Amazon’s pool of Public IPs             Private IP: 172.31.0.11 Public IP: 11.22.33.44
AZ1                      AZ2                       AZ3


## EC2 Public IP vs Private IP vs Elastic IP
*(Slide 159)*

Whenever we launch an EC2 instance, a public and a private IP get allocated to it
- Private IP: Allocated from the subnet CIDR range. Enables communication over Private Ips within the VPC.
Note: Private IP designated to an EC2 remains associated with EC2 until terminated
- Public IP: Allocated from a Amazon’s pool of available Ips. Public IP is required let you connect EC2 instance over the internet.
Note: One you Stop and Start you will lose Public IP
- Elastic IP: - It is static Public IP that can be allocated to EC2 instance. Once assigned, existing public IP gets released and replaced with the newly assigned Elastic IP.
- They are allocated to the AWS account so that we can release it from specific EC2 and re-assign it to any other EC2 instances (if needed).
Note: Elastic IP allocated to an EC2 remains associated with EC2 until terminated or released manually.


## Different ways to install & configure applications in EC2
*(Slide 160)*

- Manual – Connect over SSH/RDP and manually install the required software/applications
App
- Use AMI – Use the pre-created AMIs to launch the instance
- Use EC2 User data – Run the required installation scripts during the EC2 boot time


## EC2 User data
*(Slide 161)*

Sample User data Script
- It is a bootstrap script to automatically configure #!/bin/bash the instance at the time of first launch              # Update the instance and install Apache yum update -y
- EC2 User data script will run only once when          yum install -y httpd instance first starts. # Start Apache and enable it on boot
- It is used to automate boot tasks such as :           systemctl start httpd
  - Install OS and package updates                  systemctl enable httpd
  - Install software or agents
  - Download common files from the Internet         # Create a sample web page echo "<h1>Welcome to my EC2 instance via
- EC2 User data scripts run with a root user.           User Data!</h1>" > /var/www/html/index.html


## IAM Role
*(Slide 162)*

- An IAM role is an AWS identity that can be assumed temporarily by users, applications, or services to securely access AWS resources without using long-term credentials.
- IAM Role is used by:
  - AWS service such as Amazon EC2, Lambda to access other AWS service or resource
  - An IAM user in the same or a different AWS account (e.g. for cross-account access)
  - An external user authenticated by an external identity provider (IdP) service such as Google, Facebook to get access to AWS account
IAM user
External user IAM Role     IAM Policy AWS                                                                                           AWS Resources Services


## Exercise : Launch EC2 instance with User data
*(Slide 163)*

```text
          1        Launch EC2 (Linux) using same configurations as previous exercise. Just add the User data script below in the EC2
                   launch page -> Advanced details -> User data section
                   #!/bin/bash
                   # Install httpd (Amazon Linux 2023 version)

                   dnf update -y
                   dnf install -y httpd

                   # Start and enable the httpd service
                   systemctl start httpd
                   systemctl enable httpd

                   # Create a simple index.html file
                   echo "<h1>Hello I am loving this AWS course :)</h1>" > /var/www/html/index.html

                  https://github.com/awswithchetan/aws-solutions-architect-associate/blob/main/ec2_userdata.txt


          2    After instance is in “Running” state, open the browser and access website using URL http://<public IP>

                       Do not terminate the instance yet. We will re-use this instance for the next exercise
```


## Exercise: EC2 IAM Role
*(Slide 164)*

```text
                                                                                         Exercise steps

                We will revisit this exercise in EC2 section                1   Launch EC2 Linux instance in a default
                                                                                VPC and connect to it over SSH.


                                                                            2
                                                                                Create S3 bucket in the same region
                                                                                and upload some sample text or image
                                                                                file.

          Mumbai Region                                                         From EC2 terminal, try to download file
                                                                            3
                                                                                from S3 using AWS CLI command –
                                                                                Access denied.
                 VPC                                                            Go to AWS IAM and create IAM role for
                                   IAM Role                                 4
                                                                                EC2. Attach S3 full permissions policy to
                                                                                the role.

                                                                            5   Associate this new role to EC2 instance
                                              Upload/download
                                                                            6   Try again to download file from S3 –
                            EC2                                 S3 bucket       should be successful.

                                                                            7   Optionally copy the downloaded file into
                                                                                webserver /var/www/html/ directory and
                                                                                check if accessible over the broweser
```


## Steps
*(Slide 165)*

Pre-requisite:
1. S3 bucket and some file in the bucket
2. IAM role for EC2 instance having S3 Read or S3 Full access permissions.
1     Connect to EC2 instance over Instance Connect and try downloading file from S3 using following command
aws s3 cp s3://bucket_name/file_name .
2    Go to Instance -> Actions -> Security -> Modify IAM Role -> Select the IAM role that had created previously for EC2 instance
3    Try to download the same file again
aws s3 cp s3://bucket_name/file_name .
4     Also copy this file into webserver directory to check if you can access it over the internet
sudo cp file_name /var/www/html/
5     Access over the browser: http://EC2_PUBLIC_IP/file_name


## Exercise – EC2 AMI                                                High level steps
*(Slide 166)*

Pre-requisite:
- Running EC2 instance with WebServer installed and running
1   Select instance -> Actions -> Image and templates -> Create Image Mumbai Region
AMI                   2   Provide the name and description and Create Image
3   Wait for Image to be available Default VPC
Webserver 4   Launch a new EC2 instance from the AMI you created (Select your IAM while launching EC2 Public IP                                            instance)
Web Server    5   After instance is up and running, check if MyEC2                                 Website is accessible with new instance IP
6   Terminate both the instances


## Steps
*(Slide 167)*

```text
         1     Create an AMI
               a) EC2 console -> Select your instance -> Actions -> Image and templates -> Create image
               b) Provide Image name and description -> Create image
               c) Wait for image to be created. Check in EC2 console -> Left menu -> AMIs -> Owned by me

         2     Launch a new EC2 instance using your AMI
               a) Launch new EC2 instance but this time select your own AMI instead of Amazon Linux 2023 AMI.
                  Application and OS images -> My AMIs -> Select your AMI
               b) Select existing key pair, default VPC, existing security group (allow SSH and HTTP) and launch the
                  instance
               c) Wait for instance to be Running

         3     Verify and access the website on new EC2 instance
               a) Get the Public IP of the new EC2 instance
               b) Open the browser URL http://EC2_PUBLIC_IP


         4     Terminate both the EC2 instances
               a) Select both the instances -> Instance State -> Terminate
```


## Assignment – Copy AMI across region
*(Slide 168)*

```text
                                                               Copy AMI




                                   Mumbai Region                    N Virginia


                                                   Webserver
                                                                                   Webserver
                                                     AMI
                                                                                     AMI
                                                                             EC2
```


## EC2 Pricing options
*(Slide 169)*

Region A
1. On-Demand                                                 AZ 1                 AZ 2
2. Spot
3. Savings Plan
4. Reserved Instances
t3.xlarge          c6i.2xlarge
For EC2, you pay by per second for Amazon Linux, Windows, RHEL, Ubuntu/Pro Instances and by an hour for other operating systems
p5.4xlarge          m7.medium


## On-Demand
*(Slide 170)*

  - Default pricing option
  - No long-term commitment, no discounts                        Billed Period
  - Flexible – Any instance type, Launch / Start /Stop /                                           Billed Period Terminate at any time
  - Best for unpredictable, spiky, stateful workloads
EC2 Start            EC2          EC2 Start          EC2 Stop / Terminate                Stop / Terminate
Use cases:
- For development, Test or staging environments where uptime is unpredictable
- For POCs (short-term), experimentation and benchmarking where different instance types can be tried before deciding on Reserved instances or Savings plans
Anti-pattern:
- Long term steady-state workloads (RI or SP more suitable)
- Interruptible / stateless workloads (Spot is more suitable)


## Spot
*(Slide 171)*

- Available at up to 90% discount compared to On-demand                   EC2 Start                      Terminated pricing
- Provided from the spare capacity available                                          Billed Period
- Can be interrupted by AWS with 2 minutes of warning
- Can be used with Autoscaling group and Spot fleets
- Best for fault tolerant, non time sensitive and stateless workloads                                                             Capacity becomes 2-Min warning EC2 Requested     available Strategies for using Spot instances:
- Be flexible with the time to run the workload
- Try changing instance type and have flexibility to choose across instance families
- Try changing Availability Zone and even the region
Use cases:
- Big data, Graphics rendering, CI/CD, Machine learning training jobs
Anti-pattern:
- Critical or stateful workloads that can’t handle interruptions


## Demo – Spot instances
*(Slide 172)*

- Available at up to 90% discount compared to On-demand                   EC2 Start                      Terminated pricing
- Provided from the spare capacity available                                          Billed Period
- Can be interrupted by AWS with 2 minutes of warning
- Can be used with Autoscaling group and Spot fleets
- Best for fault tolerant, non time sensitive and stateless workloads                                                             Capacity becomes 2-Min warning EC2 Requested     available Strategies for using Spot instances:
- Be flexible with the time to run the workload
- Try changing instance type and have flexibility to choose across instance families
- Try changing Availability Zone and even the region
Use cases:
- Big data, Graphics rendering, CI/CD, Machine learning training jobs
Anti-pattern:
- Critical or stateful workloads that can’t handle interruptions


## Savings Plan (SP)
*(Slide 173)*

- Compute capacity ($/hour) is committed for 1 or 3 year
- Discounts up to 72% compared to On-demand pricing
- Pay whether you use the capacity or not                                                  Billed Period
- EC2 Savings Plan – Usage for EC2 instance family in selected Region (max discount)
- Compute Savings Plan – Usage across any EC2, c6i.2xlarge Lambda and Fargate in any region                                        Reserve                              End of Capacity      (in AZ2, Region A)   1 or 3 year
- Can pay Full upfront (max discount), Partial upfront or No upfront
- Best for steady state and predictable workloads
Use cases:
- Databases, Steady-state workloads
- Having mix of compute workloads including EC2, Lambda and Containers
Anti-pattern
- Short-term workloads like Development/Test environment, Proof of Concept, Pilot workloads, Weekly or monthly batch jobs Max discount = EC2 Savings Plan + 3 Year Term + Full Upfront payment


## Reserved Instances (RI)
*(Slide 174)*

  - Long-term commitment for given Instance type/size
  - Discounts up to 72% compared to On-demand pricing Billed Period
  - EC2 capacity is reserved for 1 or 3 year term
  - Pay whether you use the capacity or not
  - Standard RI – Fixed instance type, AZ (max discount)
  - Convertible RI – Can change instance type, AZ or Region c6i.2xlarge             End of
  - Can pay Full upfront, Partial upfront or No upfront       Reserve 1 or 3 year Capacity    (in AZ2, Region A)
  - Best for steady state and predictable workloads
Use cases:
- Databases, Steady-state applications
Anti-pattern:
- Short-term workloads like Development/Test environment, Proof of Concept, Pilot workloads, Weekly or monthly batch jobs


## How to compare pricing?
*(Slide 175)*

*AWS Pricing Calculator*


## EC2 Pricing options comparison
*(Slide 176)*

On-Demand                                   Spot                          Savings Plan                Reserved Instances
- No commitment
- No commitment
- 1-year or 3-year
- 1-year or 3-year
- Pay by second
- Up to 90% discount                 commitment for $/hour           commitment for EC2
- Start or Stop/Terminate at
- Instance can be terminated
- Up to 72% discount              instance type/hour with 2-minute warning
- 3 payment options:
- Up to 72% discount
- Lowest cost
- Billed only when instance                                                    Full upfront
- 3 payment options:
- Suitable for fault tolerant, is in Running state                                                          Partial Upfront                 Full upfront non-time sensitive and                                                Partial Upfront No upfront
- Highest cost Stateless workloads            •   Pay even if not using the          No upfront
- Suitable for capacity                    •   Pay even if not using the unpredictable, spiky,
- EC2 Savings Plan                capacity stateful workloads
- Compute Savings plan
- Standard RI
- Suitable for steady state
- Convertible RI
and predictable workloads •     Suitable for steady state and predictable workloads


## Pricing example with different EC2 pricing models
*(Slide 177)*

```text
         m5.large (2vcpu, 8GiB RAM) in N. Virginia region

                                              Pricing model                       Price (Per Hour)
            On-Demand                                         $0.096
            Spot Instance (Spot Price)                        $0.038 - $0.04 (up to 60% off as per current trend)
            Reserved Instance (1 year)                        $0.060 (No Upfront) - $0.057 (All Upfront)
            Reserved Instance (3 years)                       $0.041 (No Upfront) - $0.036 (All Upfront)
            Reserved Convertible Instance (1 year)            $0.071 (No Upfront) - $0.066 (All Upfront)
            EC2 Savings Plan (1 year)                         $0.060 (No Upfront) - $0.056 (All Upfront)
            EC2 Savings Plan (3 year)                         $0.041 (No Upfront) - $0.036 (All Upfront)
            Compute Savings Plan (1 year)                     $0.071 (No Upfront) - $0.066 (All Upfront)
            Compute Savings Plan (3 year)                     $0.049 (No Upfront) - $0.044 (All Upfront)
            Dedicated Instance (Instance/hr + $2/hr/region)   $0.102 + $2/hr/region = $2.102 (On-Demand Price)
            Dedicated Host (m5 host) – (96 vCPU/48 core)      $5.069 (On-Demand Price)
            On-Demand Capacity Reservations (ODCR)            $0.096 (On-Demand Price)
```


## EC2 On-demand Capacity Reservation (ODCR)
*(Slide 178)*

- Reserve EC2 capacity in a specific Availability Zone for any duration.
- Create ODCR anytime without entering a 1-year or 3-year term commitment.
- Cancel the ODCR anytime to release the capacity and stop incurring charges.
- For ODCR – specify AZ, number of instances with instance attributes – type, size, platform, tenancy.
- By default, charged at EC2 On-Demand rate. RI and SP discounts applies to matching capacity.
- Suitable for business-critical workloads that require a capacity assurance.
AZ
EC2 Start of ODCR        Capacity is reserved   End of ODCR


## EC2 Tenancy
*(Slide 179)*

- Tenancy determines how your EC2 instances are hosted on the underlying physical hardware.
- It defines whether your instances share hardware with other AWS customers or are isolated on dedicated servers.


## EC2 Tenancy – Shared vs Dedicated
*(Slide 180)*

Region A
Shared Tenancy                              AZ 1               AZ 2 Customer #1
- Underlying physical host is shared across different AWS customers (AWS account)
- This is a default tenancy for VPC and       Customer #2
EC2                                                       Host 1              Host 2
Customer #3
Host 3              Host 4


## EC2 Tenancy – Shared vs Dedicated
*(Slide 181)*

Region A Dedicated Tenancy AZ 1               AZ 2
- Underlying physical host is dedicated for a Customer #1 single customer (AWS account)
- Used for Regulatory and Compliance requirements
- On-demand, Spot and Reserved pricing is Customer #2    X Host 2 available                                                        Host 1
- There are 2 options for Dedicated Tenancy:                   X o      Dedicated Instances                Customer #3 o      Dedicated Hosts                                   X Host 3              Host 4 Customer #4


## EC2 Dedicated Instances
*(Slide 182)*

Region A Dedicated Instances                                 Launch dedicated    AZ 1               AZ 2 instance
- Instance is launched on a host which is               Customer #1
dedicated for the customer, but customer do not decide on which host to launch the instance
- Do not get access, placement choice or visibility Customer #1 of the underlying host                                                                              Host 2 Host 1
- There are 2 pricing elements: o      EC2 per hour running charge                                X o      Dedicated host per hour charge ($2/hour) o      On-demand, Spot, Reserved                  Customer #2
Host 3              Host 4


## EC2 Dedicated Host
*(Slide 183)*

Region A Dedicated Host AZ 1               AZ 2
- Allow to use existing per-socket, per-core, or       Customer #1
software licenses tied to the physical host
- Host affinity where you can choose this host to launch EC2 instances                              Customer #2   X
- Pricing                                                                                    Host 2 Host 1 o     Per host instead of per instance billing o     On-demand, Reserved, Savings Plan Customer #3
Host 3              Host 4 Customer #4


## EC2 Instance Tags
*(Slide 184)*

Key       Value
- Tags are in the form of Key Value Pairs. Name = WebServer
- Can be useful to filter resources while queries AWS resources. Owner = Chetan
- Are useful when you use AWS deployment services like Code deploy, Code pipeline. Project = SpaceMission EC2 Environment = Prod
- Can use tags in IAM policies to restrict access to users for a particular instance(s).                                                                      DO_NOT_STOP
- Very useful for cost allocation e.g. per project or environments etc.


# EC2 Advanced

*(Source: Slide 185)*

*For Solutions Architect*


## EC2 Advanced section – For Solutions Architect
*(Slide 186)*

- EBS Deep dive
- AMIs vs EBS Snapshot vs EC2 User data
- Elastic Network Interface (ENI)
- EC2 Hibernation
- EC2 Placement groups
- EC2 Instance Metadata Service (IMDSv2)


## Elastic Block Storage
*(Slide 187)*


## EBS – Key terminologies
*(Slide 188)*

- Volume Size - The storage capacity of an EBS volume, measured in GiB (Gibibytes).
- IOPS (Input/Output Operations Per Second)
  - Measures how many read and write operations per second a volume can handle
  - Indicates the speed and responsiveness of storage. Important for transaction-heavy workloads (e.g. databases).
- Baseline IOPS
  - The default guaranteed performance for your volume type (before bursting or provisioning).
- Provisioned IOPS (PIOPS)
  - A feature where you explicitly set desired IOPS (up to 256,000).
- Burst Performance / Burst Credits
  - Temporary performance boost above baseline IOPS when needed.
  - Volumes accumulate burst credits when idle and spend them under load.
  - Helps small volumes handle occasional heavy I/O spikes.
- Throughput (MB/s)
  - Measures the amount of data transferred per second, typically in MB/s.
  - IOPS = “how many”; Throughput = “how fast”.


## EBS fundamentals
*(Slide 189)*

AZ
- An Amazon EBS is a block-level storage device that you can attach to your EC2 instance in a given AZ.
- EBS provides high availability, reliability and durability for the                EC2 data stored in EBS volumes 99.9% Availability (within AZ)
- Dynamically increase size, modify the provisioned IOPS                      99.999% Durability capacity, and change volume type for existing volumes.
- Allows encryption using encryption keys from AWS Key                 1GiB                       64TiB Management System (KMS).
- EBS volumes data can be backed up using point-in-time snapshots. Snapshots are stored in S3.                                        Data at rest encryption
Backup
snapshot


## EBS volume types
*(Slide 190)*

```text
                                                                           Performance
                                                                         gp3 - 3000 baseline IOPS, scales up to 80k IOPS
                                                 General Purpose SSD     gp 2 - 3 IOPS x per GB, scales up to 16k IOPS


                                    gp3, gp2
                                                    Provisioned IOPS
                                                           SSD
                                                                           64k to 256k IOPS
           SSD
                                     io1, io2 Block Express



                                                  Throughput Optimized
                                                         HDD
                                          st1                               250 to 500 IOPS



            HDD                                       Cold HDD

                                          sc1
```


## EBS volume types
*(Slide 191)*

Use cases
- Operating system General Purpose SSD
- Relational databases: MySQL, SQL Server, PostgreSQL, SAP, Oracle gp3, gp2 Provisioned IOPS SSD            •   NoSQL Databases SSD                                                           •   Cassandra, MongoDB, CouchDB io1, io2 Block Express
- Big Data , Analytics Throughput Optimized   •   Kafka, Splunk, Hadoop, Data Warehousing HDD st1
- File / Media CIFS/NFS HDD                                       Cold HDD           •   Transcoding, Encoding, Rendering
sc1


## EBS volume types
*(Slide 192)*

EBS Volume
Solid State Drive                                           Hard Disk Drive (HDD) (SSD) Volumes                                                     Volumes
Provisioned IOPS                       General Purpose              Throughput Optimized Cold HDD Volumes SSD Volumes                            SSD Volumes                    HDD Volumes (sc1) (io1, io2 Block Express)                   (gp3 and gp2)                      (st1)
- Highest performance
- Balance price vs
- Throughput optimized
- Throughput optimized
- Configurable I/O (> 10k)                 performance
- Sequential I/O
- Infrequent access
- For critical business
- Burstable I/O
- Big data, Data Warehouse
- For wide variety of general
- Log processing SAP systems.                             purpose workloads
High                                                       Performance & Cost                                           Less


## EBS Performance – good to know
*(Slide 193)*

CloudWatch Monitoring
- EBS volume performance like IOPS, throughput, latency etc. can be monitored using Amazon CloudWatch.
- Typically, CW metrics like VolumeReadOps / VolumeWriteOps / VolumeReadBytes / VolumeWriteBytes/ VolumeQueueLength etc. are used for troubleshooting performance bottlenecks
EBS-Optimized Instances:
- Amazon EBS-Optimized instances use an optimized configuration stack and provide additional, dedicated bandwidth for Amazon EBS I/O.
- This optimization provides the best performance for your EBS volumes by minimizing contention between Amazon EBS I/O and other traffic from your instance.


## EBS Snapshots
*(Slide 194)*

- Snapshots are point in time backup of EBS Volume.
- Snapshot are stored in S3 and are highly durable within a region.
- Snapshots are incremental – One full snapshot + changed data only
- Can be shared with other AWS Accounts and can be copied to other AWS Regions
- You can delete snapshots and retain it in Recycle Bin for a desired retention period
- Amazon Data Lifecycle Manager (DLM) automates EBS snapshot creation and retention.
Create Snapshot
EC2 Instance                      EBS Volume EBS Snapshot            Recycle Bin


## EBS Fast Snapshot Restore
*(Slide 195)*

- Amazon EBS fast snapshot restore (FSR) enables you to create a volume from a snapshot that is fully initialized at creation.
- This eliminates the latency of I/O operations on a block when it is accessed for the first time.
- Volumes that are created using fast snapshot restore instantly deliver all of their provisioned performance.
EBS Volume
Create Snapshot
EC2 Instance                       EBS Volume EBS Snapshot


## EBS Encryption
*(Slide 196)*

- Amazon EBS supports encryption using AWS KMS (Key Management System).
- You encrypt EBS volumes by enabling -
1. Encryption by default. This is a region level setting. In every region a default KMS key is created with alias aws/ebs
2. Manually when creating the volume. Uses customer managed Encryption (CMK) keys from KMS.
- When you create an encrypted EBS volume and attach it to a supported instance type, the following types of data are encrypted:
  - Data at rest inside the volume
  - All data moving between the volume and the instance
  - All snapshots created from the volume
  - All volumes created from those snapshots
- No performance impact for encrypted volumes. EC2 EBS            EBS           New EBS Volume        Snapshot         Volume


## EBS Encryption - scenarios
*(Slide 197)*

- The snapshot of encrypted volumes are also encrypted and to create a new volume from the snapshot the access to encryption key is required.
- You can't directly encrypt existing unencrypted volumes or snapshots. To encrypt an unencrypted volume, create a snapshot of that volume, and then use the snapshot to create a new encrypted volume.
- If a Customer-Managed Key (CMK) is disabled, the encrypted EBS volume becomes temporarily inaccessible. If the CMK is deleted, it results in permanent data loss, as the data can no longer be decrypted.
Create Snapshot                  Create Volume
KMS New EBS EBS Volume                 EBS Snapshot                    Volume


## EBS Multi Attach
*(Slide 198)*

- EBS supports a feature called Multi-Attach for io1 and io2 Block     Region Express volumes (Provisioned IOPS)
- Multi-Attach allows a single EBS volume to be shared by up to 16 EC2 instances (Linux) within the same Availability Zone
- Each EC2 instance has full read/write access.                                 Availability Zone
- Use case:
  - Highly available clustered applications (Oracle RAC, SAP)
  - Zero downtime applications
  - Analytics or rendering applications where nodes process shared input/output files in parallel.
Key considerations:
- Cannot be used as a EC2 boot volume.
- Applications must handle concurrent writes safely to maintain data            EBS Multi-attach consistency.


## EBS – Good to know
*(Slide 199)*


## EBS volume AZ considerations
*(Slide 200)*

Availability Zone 1             Availability Zone 2
- EBS volumes resides in given AZ Root
- EBS volumes are automatically replicated within an AZ EC2-A                           EC2-C for high availability.                                                          Data
- EBS volume can be detached and attached to another                         EBS
instance in the same AZ
- For moving EBS volume across the AZs, create a           EC2-B
snapshot of volume and then create a new volume from Create the snapshot in another AZ                                           snapshot


## Delete on Termination Attribute
*(Slide 201)*

Availability Zone 1
- EBS volume attribute DeleteOnTermination defines whether volume is deleted or retained when EC2 instance terminates                                                  Root
EC2
- By default, the root EBS volume is deleted (DeleteOnTermination = true) Data
- By default, any other (non-root) EBS volume is not deleted                               EBS (DeleteOnTermination = false)
- This attribute value can be controlled by the user (AWS console / CLI)


## EC2 Image Builder
*(Slide 202)*


## Process to create a custom AMI
*(Slide 203)*

```text
                        EBS                                           EBS                                     EBS
                      Snapshots                                      Volume                                 Snapshots

                                              Create volume(s)                          Creates snapshots


                                                                  attach      persist
                                 contains                                    changes        Create AMI            contains


                                              Launch
                                                                       EC2

                         AMI                                                                                   AMI
                        (base)                                                                               (custom)

                                                                   Install packages,
                                                                 software, Applications
```


## EC2 Image Builder
*(Slide 204)*

- Automate the creation, management, and deployment of AMIs
- Configure pipelines to automate updates and system patching for the images.
- Can be run on a schedule basis (weekly, whenever packages are updated).
- Works across AWS regions and AWS Accounts
- Free service
Builder EC2                        Test EC2 EC2 Image                               Instance      AMI                 Instance Builder Build components are                         Test the       Distribute AMI applied through recipes                      application    across Regions


## Elastic Network Interface (ENI)
*(Slide 205)*


## Elastic Network Interfaces (ENI)
*(Slide 206)*

- Logical component in a VPC that represents a virtual network card Availability Zone
- EC2 instance has one primary ENI and can have multiple Eth0 – primary ENI secondary ENIs depending on instance type 192.168.0.31
- The ENI can have the following attributes:                        EC2 Eth1 – secondary ENI
  - Primary private IPv4, one or more secondary IPv4                      192.168.0.42
  - One Elastic IP (IPv4) per private IPv4
  - One Public IPv4                                                        Can be moved
  - One or more security groups
  - A MAC address                                                         Eth0 – primary ENI EC2
- You can create ENI independently and attach or move them across EC2 instances
- ENIs are AZ-scoped. Can’t move across AZs.
- Each ENI can have its own security group, IP, and routing.


## ENI Use cases
*(Slide 207)*

High Availability / Failover within an AZ
- Move an ENI (and its private IP) from one instance to another to achieve quick failover. Static IP retention
- Detach the ENI from a terminated or failed instance and attach it to a new one to retain the same private IP and MAC.
- Helps avoid DNS updates or reconfiguration. Multi-Network (Multi-Homed) Instances
- Attach multiple ENIs to a single EC2 instance, each connected to a different subnet or security group.
- Enables segregation of traffic (e.g., app network vs. database network).
- Assign a secondary ENI specifically for administration or monitoring traffic.


## ENI Use cases
*(Slide 208)*

- High Availability solution by attaching ENI to hot standby instance in case of failure
Virtual private cloud (VPC)                       Internet gateway 10.10.0.0/16
Availability Zone Subnet A                                     10.10.0.0/24
X               ENI Private IP 10.10.0.15 Elastic IP 31.23.45.67


## ENI Use cases
*(Slide 209)*

- Creating Management Network
- Creating a dual home instances Internet gateway Corporate data Virtual private cloud (VPC)                                       10.10.0.0/16         center (on-prem) Availability Zone
Subnet A      10.10.0.0/24          Subnet B   10.10.1.0/24
192.168.0.0/16
VPN
ENI                         ENI Private IP 10.10.0.15               Private IP10.10.1.30 Elastic IP 31.23.45.67


## EC2 Hibernation
*(Slide 210)*

- EC2 supports hibernation (suspend-to-disk) for supported OS and AMI types
- Hibernation saves the contents from the instance memory (RAM) to EBS root volume.
- When the instance is started again:
  - The EBS root volume is restored to its previous state
  - The RAM contents are reloaded
  - The processes that were previously running on the instance are resumed
  - Previously attached data volumes are re-attached and the instance retains its instance ID
- Use case: Saves cost by not paying for compute while preserving RAM state for long-running applications e.g. analytics workload, VDI, development workstations etc.
Stopping                   Stopped
Hibernate                                                   Start EC2                           EC2                        EC2                              EC2 RAM                            RAM                                                         RAM
Encrypted Root                                      RAM Volume                        Hibernation


## EC2 Instance metadata service (IMDSv2)
*(Slide 211)*

- IMDSv2 allows applications and users on an EC2 instance to retrieve metadata about the instance like instance ID, AMI ID, IPs, security groups, IAM role credentials.
- Used by applications or scripts for automation, configuration, and identity awareness.
- Service runs on 169.254.169.254 IPv4 address and [fd00:ec2::254] IPv6 address
> export TOKEN=$(curl -X PUT -H "X-aws-ec2-metadata-token-ttl-seconds: 300" http://169.254.169.254/latest/api/token)
> curl http://169.254.169.254/latest/meta-data/ -H "X-aws-ec2-metadata-token: $TOKEN"
Remember this IP address for IMDS service: 169.254.169.254


## EC2 Placements groups
*(Slide 212)*

- A placement group is a logical grouping of EC2 instances within an Availability Zone
- It lets you control how instances are placed on AWS infrastructure to meet performance, latency, compliance needs and avoiding system wide failures at hardware level
- AWS offers three placement strategies:
  - Cluster placement group
  - Spread placement group
  - Partition placement group


## EC2 Placement groups
*(Slide 213)*

Cluster Placement group           Spread Placement group                  Partition Placement group
- Packs instances close together
- Strictly places a small group of
- Spreads instances across logical inside an AZ.                        instances across distinct                partitions
- Enables workloads to achieve the     underlying hardware
- Groups of instances in one low-latency network performance •    Reduces correlated failures.             partition do not share the for tightly-coupled node-to-node •   Ideal for applications that have a       underlying hardware with groups of communication                        small number of critical                 instances in other partitions.
- Ideal for high-performance           instances to reduce the risk of
- Ideal for large distributed and computing (HPC) applications.        simultaneous failures in the             replicated workloads, such as underlying hardware                      Hadoop, Cassandra, and Kafka.


# Elastic Load Balancer and Autoscaling Group

*(Source: Slide 214)*

### Load Balancing and Autoscaling


## Web
*(Slide 215)*

```text
                                                        Users                                 Browser
         myapp.com on AWS                                                                                                     CloudFront

                                                         Route53            myapp.com                                                             Edge
                                                                                                                                                Locations



                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                     Lambda

                                                Web                                                                             Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                               Convert
                                                                    S           S                Rekognition             S3                S3
                            SNS
                                                                                                           AI models                                    QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker


                            SQS
                                              ElastiCache

                                                                               Neptune        Kinesis                    S3              EMR
                                                                                                                                                            Redshift
                       CloudWatch                  Multi-AZ




                                                              RDS       DynamoDB              Glue
```


## High Availability and Scaling
*(Slide 216)*

- High Availability means your application runs despite failure in some of the components and during increased traffic
- In AWS world High availability generally refers to using multiple AZs
- Scalability means ability to scale the capacity up or down as per demand
  - Horizontal scaling is used in distributed systems - Web applications, Big data processing
  - Vertical scaling is used in non-distributed systems - Relational databases
- High Availability and Scalability go hand-in-hand
- Use Elastic Load Balancer (ELB), Auto Scaling group (ASG) for achieving High Availability and Scalability


## High Availability
*(Slide 217)*

```text
                                              Region



                                                         Availability Zone       Availability Zone




                                                       EC2               EC2   EC2               EC2

       Auto Scaling Group
```


## Scaling
*(Slide 218)*

```text
            Option 1: Vertical Scaling



                                                                Scale up
                                                                              Change
                                                                           Instance size
                                                         Stop                 & Start                       Vertical
                                               EC2                EC2                        EC2           Scaling has
                                                                                                              limit
                                              m5.large
                                  (2 vcpu, 8 GB RAM)
                                                                                           m5.24xlarge
                                                                Scale down             (96 vcpu, 384 GB RAM)
```


## Scaling
*(Slide 219)*

```text
            Option 2: Horizontal Scaling
                                                          Scale Out




                                              EC2   EC2               EC2   EC2



                                                          Scale In
```


## Elastic Load Balancer
*(Slide 220)*


## AWS Elastic Load Balancer
*(Slide 221)*

User/Client
- Load Balancer distributes incoming traffic to multiple downstream servers:
  - EC2 instances                                                          Availability Zone            Availability Zone
  - Containers                                                                                                      SSL/TLS xxxx.elb.amazon.aws.com
  - IP addresses
  - Lambda functions ELB
- It’s a regional service. High availability across Availability zones (AZs)
- Supports protocols like HTTP, HTTPS, HTTP/2, TCP, UDP, gRPC
- Expose a single point of access (DNS) to your application Application Server             Application Server
- Supports Dual-stack mode (IPv4 and IPv6 traffic)
- Seamlessly handle failures of downstream instances by performing health checks to the instances
- Provide SSL/TLS termination for your websites and web applications


## Certificate
*(Slide 222)*

```text
                                                                                              Authority (CA)
      HTTPS and SSL/TLS
                                              HTTPS Traffic


     https://somebank.com                                                                    somebank.com




                                                              https://youtu.be/cLYv4uSFJA8



                                                                               Public Key


                                                                                                  Private Key
```


## Elastic Load Balancer                                            yourdomain.com
*(Slide 223)*

ELB IP addresses
- ELB integrates with many other AWS services                                      User/Client Route 53                                                             ACM
  - Auto Scaling Groups
  - Amazon Route 53                                     Availability Zone                 Availability Zone
  - AWS Certificate Manager (ACM)                                                                     SSL/TLS
  - Amazon ECS
  - Amazon CloudWatch
  - AWS WAF                                                                    ELB
  - AWS Global Accelerator
Application Server                Application Server


## Elastic Load Balancer components
*(Slide 224)*

- Listener: Supports multiple listeners on different ports
- Target Group: group of targets
- Target: EC2 instance, Container, IP , Lambda functions
80 8080 443            Listeners
Target Groups
Target Group 1                 Target Group 2
Targets
EC2        EC2 Instance   Instance Container   Container


## Security Groups
*(Slide 225)*

```text
                                              HTTPS / HTTP                     HTTP Restricted
                                              From anywhere    LB SG           to Load balancer
                                                                                                  EC2
                              End user
                                                             LOAD BALANCER
             Load Balancer Security Group (sg-0bc494c71e6cf1896):




           Application Security Group: Allow traffic only from Load Balancer
```


## Load Balancer types
*(Slide 226)*


## Types of Elastic Load Balancers
*(Slide 227)*

Internet
AWS offers following types of Load Balancers       Region
10.100.0.0/16
1. Application Load Balancer (ALB) – Layer 7 HTTP, HTTPS, WebSocket, HTTP/2, gRPC                   Availability Zone                 Availability Zone
Public subnet                   Public subnet
2. Network Load Balancer (NLB) – Layer 4 TCP, TLS (secure TCP), UDP
3. Gateway Load Balancer (GWLB) – Layer 3                                      ELB
GENEVE Protocol on IP                                Private subnet                  Private subnet
4. Classic Load Balancer (CLB) – Layer 4 & 7 HTTP, HTTPS, TCP, SSL/TLS (secure TCP)                 Application Server              Application Server


## OSI Network Layers
*(Slide 228)*

```text
                            Client                                                                                                           Server



         7       Application                   Application
                                                                            HTTP, HTTPS, HTTP/2, gRPC                              7    Application
                                               Load Balancer

        6       Presentation                                                                                                       6   Presentation

         5             Session                                                                                                     5       Session

                                              Network                     TCP, UDP
         4          Transport                                                                                                      4     Transport
                                              Load Balancer

         3           Network                  Gateway                                                                              3      Network
                                                                                 IP
                                              Load Balancer

         2            Datalink                                                                                                     2       Datalink

         1            Physical                     Electronic / Light / Radio signals via Ethernet cable or optical fiber or Air   1       Physical
```


## Application Load Balancer
*(Slide 229)*


## Application Load Balancer (ALB)
*(Slide 230)*

- Operates at Layer 7
- Supported protocols HTTP, HTTPS, WebSocket, HTTP/2 and gRPC
- Supports multiple listeners e.g. http, https, custom port                                     TLS certificate
- Load balancing to multiple Target groups which contains targets 80 8080 443            Listeners across EC2 instances, container tasks, IP addresses & Lambda functions                                                                         Target Groups
- Supports SSL/TLS termination                                          Target Group 1                 Target Group 2
- Supports Target Group Weighting (Blue/green, A/B testing) Targets
- Supports authentication using Amazon Cognito and federation (AD, OIDC, SAML etc.)
- Supports request routing - Host based, URL/path based, source-ip,     EC2        EC2 Instance   Instance Container   Container http-header, query-string


## ALB -> Listener -> Rules -> Target group -> Targets
*(Slide 231)*

```text
                                                                     ALB



                       Listener 1 (port x)                                                         Listener 2 (port y)

                          default rule                                           rule 1                         rule 2           default rule

                                                                       Weighted Target Groups:
                                                       Routing: path based, host based, query string, source-ip etc.


                        Target Group 1                                  Target Group 2                  Target Group 3          Target Group 4

                                                                     Round robin, Least outstanding,
                                                                             Sticky session
  Health Check




              Target           Target         Target                    Target            Target       Target          Target   Target     Target
```


## Application Load Balancer – Advanced Features
*(Slide 232)*

- HTTP to HTTPS redirection
- Support for returning custom HTTP responses or fixed responses TLS certificate
- Sticky sessions
  - Duration based cookies (Cookie name: AWSALB)
  - Application based cookies (Cookie name: AWSALBAPP)                    80 8080 443            Listeners
- Connection Draining / deregistration delay (default: 300 sec)                  Target Groups
- Dual Stack DNS – Supports both IPv4 and IPv6 traffic                                              Target Group 2 Target Group 1
- Dynamic port mapping for Container targets
- Mutual TLS Authentication (X.509 certificates)                                         Targets
- Preserves Client IP: X-Forwarded-For
- Request Tracing: X-Amzn-Trace-Id                                   EC2        EC2 Instance   Instance Container   Container


## Application Load Balancer – Advanced Features
*(Slide 233)*

- Content Based Routing                                                 HTTP Request
  - Host-based routing (api.example.com)
  - Path-based routing (/images/*)
  - HTTP method based(GET, POST)
  - Query string parameter-based (/app?version=v2) asia.example.com                   europe.example.com
  - Source IP CIDR-based                                           Listener
Target Group 1                         Target Group 2
Target Groups


## Application Load Balancer – Advanced Features
*(Slide 234)*

- Content Based Routing                                                      HTTP Request
  - Host-based routing (api.example.com)
  - Path-based routing (/images/*)
  - HTTP method based(GET, POST)
  - Query string parameter-based (/app?version=v2) https://example.com/asia                   https://example.com/europe
  - Source IP CIDR-based                                                Listener
Target Group 1                            Target Group 2
Target Groups


## Application Load Balancer – Advanced Features
*(Slide 235)*

- Content Based Routing                                                         HTTP Request
  - Host-based routing (api.example.com)
  - Path-based routing (/images/*)
  - HTTP method based(GET, POST)
  - Query string parameter-based (/app?version=v2) https://example.com/location
  - Source IP CIDR-based                   https://example.com/location    Listener             ?region=europe ?region=asia
Target Group 1                                Target Group 2
Target Groups


## Application Load Balancer – Advanced Features
*(Slide 236)*

- Operates at Layer 7 (Application Layer)
- Supported protocols HTTP, HTTPS, WebSocket, HTTP/2 and gRPC
- HTTP to HTTPS redirection
- Support for returning custom HTTP responses or fixed responses
- Sticky sessions - Duration based (Cookie: AWSALB) , Application based (Cookie: AWSALBAPP)
- Connection Draining / deregistration delay (default: 300 sec)
- Dual Stack DNS – Supports both IPv4 and IPv6 traffic
- Dynamic port mapping for Container targets
- Mutual TLS Authentication (X.509 certificates)
- Preserves Client IP: X-Forwarded-For
- Request Tracing: X-Amzn-Trace-Id


## ALB Exercises
*(Slide 237)*


## Pre-requisites
*(Slide 238)*

- Public Domain name and Route 53 – Public Hosted Zone
- EC2 Launch Template


## EC2 Launch Templates
*(Slide 239)*

- An easy way to define the settings required to launch EC2 instances
- We can define EC2 launch parameters like
  - Amazon Machine Image (AMI) ID,
  - Instance type
  - Key pair
  - Security groups
  - Use data
  - And other configuration parameters.
- Supports versioning


## Launching EC2 instances
*(Slide 240)*

Internet
Region
- Default VPC Availability Zone             Availability Zone
- Different Subnets (for choosing different AZs)
- Same Security Group Public subnet                Public subnet
- Same SSH Key-pair
- Different website content (User data)
ALB TG
Web Server                  Web Server


## Exercise – Create EC2 launch templates
*(Slide 241)*

- Let’s first create a Security group so that we can reuse it (Optional) and node down default VPC ID.
- Now let’s create a Launch Templates for Webservers
  - We want different index.html for both the webservers. Hence, we will create 2 separate launch templates using following User data scripts
  - Make sure to use different AZ (subnets) #!/bin/bash                                                           Launch Template 1 – User data dnf update -y dnf install -y httpd systemctl start httpd systemctl enable httpd echo "<h1>You have reached a website for Asia</h1>" > /var/www/html/index.html #!/bin/bash                                                           Launch Template 2 – User data dnf update -y dnf install -y httpd systemctl start httpd systemctl enable httpd echo "<h1>You have reached a website for Europe</h1>" > /var/www/html/index.html


## Exercise – ALB with two backend EC2 instances
*(Slide 242)*

```text
                                                            Internet                                  High level steps

                                                                                    1   Launch Web server 1 and Web server 2 using the
                       Region
                                                                                        corresponding Launch Templates.
                                  Availability Zone             Availability Zone


                                  Public subnet                Public subnet        2   Create ALB Target group and add both EC2 instances.
                                                                                        Configure health check on port 80 with /index.html


                                                                                    3   Create ALB with HTTP listener and forward the traffic
                                                                                        to Target group. Open ALB Security group for HTTP
                                                                                        traffic.
                                                      ALB
                                                      TG
                                                                                    4   Update EC2 security group to allow HTTP traffic from
                                                                                        ALB security group

                                     Web Server                  Web Server         5   Access ALB over the browser using AWS provided ALB
                                                                                        DNS. Hit multiple times.
```


## Exercise – ALB with custom domain name
*(Slide 243)*

yourdomain Route53 Public Hosted Zone                                            Internet                        Continuing with earlier setup ALB DNS
Region                                                       Pre-requisite for this exercise: Availability Zone             Availability Zone   • You should have your public domain-name and DNS should be pointing to Route53 public hosted zone Public subnet                Public subnet
- Refer Labs prerequisites section if you haven’t done this earlier.
1    In Route53 Public hosted zone create an A (Alias) record and point it to ALB DNS ALB TG 2    Wait for some time and access application using your custom domain name
Web Server                  Web Server


## Exercise – Enable HTTPS (TLS termination)
*(Slide 244)*

```text
                                     yourdomain
    Route53 Public
     Hosted Zone
                                      ALB IPs                                                            Continuing with earlier setup
                                                               Internet



                       Region                                                                      1   Get TLS certificate from Amazon ACM for
                                                                                                       your domain name. Validate the domain
                          VPC                                                        10.0.0.0/16       ownership through ACM portal.

                                                  Availability Zone
                                                                                                   2   Modify ALB listener to HTTPS (443) and
                                Public subnet                     Public subnet                        associate TLS Certificate. Forward traffic
                                                               Enable HTTPS                            to same target group as earlier.

                                                        ALB                       10.0.1.0/24      3   Update ALB security group to allow
                           10.0.0.0/24
                                                         TG                                            HTTPS (443) traffic
                                 Private subnet                   Private subnet


                                                                                                   4   Access website using https://yourdomain
                                     Web Server                       Web Server
                          10.0.11.0/24                                             10.0.12.0/24
```


## Exercise – ALB Sticky session
*(Slide 245)*

```text
                                     yourdomain
    Route53 Public
     Hosted Zone                                            Internet                      Continuing with earlier setup
                                      ALB IPs


                       Region
                                                                                    1   Update target group attribute to enable
                                  Availability Zone             Availability Zone
                                                                                        Session stickiness for 30 seconds
                                                                                        duration
                                  Public subnet                Public subnet

                                                                                    2   Try accessing website with your custom
                                                                                        domain name, refresh couple of times,
                                                                                        you should see that you land onto the
                                                                                        same EC2 instance. Wait for 30 seconds
                                                                                        and try again.
                                                      ALB
                                                      TG




                                     Web Server                  Web Server
```


## Cleanup – Very important
*(Slide 246)*

1. Terminate both the EC2 instances
2. Delete Load Balancer Target groups
3. Delete Load Balancer
4. Delete Route53 records


## Assignment – ALB path-based routing
*(Slide 247)*

```text
                                     yourdomain
    Route53 Public                                                              1   Create two new EC2 launch templates using the
     Hosted Zone
                                      ALB IPs                                       User data scripts provided in the next slide. (We
                                                                                    need /asia and /Europe directories this time)
                       Region
                                                                                2   Launch Web server 1 and Web server 2 in
                                  Availability Zone         Availability Zone       respective subnets using Launch Templates
                                                                                    created above for Asia website and Europe
                                  Public subnet             Public subnet           website. Security group to allow HTTP traffic.

                                                                                2   Create 2 target groups and register one EC2
                                                                                    instance in each target group as per the intended
                                                                                    traffic routing (Webserver 1 for /asia and
                                                                                    Webserver 2 for /europe).

                                                      ALB                       3   Create Load Balancer listener rules to route
                                                      TG                            traffic to respective Target groups with matching
                                                                                    paths i.e. /asia to TG1 and /europe to TG2

                                                                                4   Access web application with domain name and
                                     Web Server               Web Server            append path /asia. Traffic should be routed to web
                                                                                    server for Asia. Do the same for /europe path and
                                                                                    traffic should be routed to the webserver for Europe.
```


## User data for Launch Templates
*(Slide 248)*

```text
         #!/bin/bash
         dnf install httpd -y
         systemctl start httpd.service
         systemctl enable httpd.service
         echo "<h1>This is a Webserver for Asia</h1>" > /var/www/html/index.html
         mkdir /var/www/html/asia
         echo "<h1>This is a Webserver for Asia</h1>" >
         /var/www/html/asia/index.html

         #!/bin/bash
         dnf install httpd -y
         systemctl start httpd.service
         systemctl enable httpd.service
         echo "<h1>Hello to the world ! </h1>" > /var/www/html/index.html
         mkdir /var/www/html/europe
         echo "<h1>This is a Webserver for Europe</h1>" >
         /var/www/html/europe/index.html
         echo "Configured successfully"
```


## Cleanup – Very important
*(Slide 249)*

1. Terminate both the EC2 instances
2. Delete Load Balancer Target groups
3. Delete Load Balancer


## Network Load Balancer
*(Slide 250)*


## Network Load Balancer
*(Slide 251)*

- Operates at Layer 4
- Supported protocols TCP, UDP, and TLS                                                                                          Clients
- Targets: Instance, IP, Containers and Application Load Balancer (Lambda not supported)
- Handle millions of requests per second. Region
- NLB has one static IP per AZ, and supports assigning Elastic IPs
  - Useful for IP whitelisting on the client side
- Sticky Sessions:
    - Supported using the client’s source IP address.
    - Uses Flow Hash algorithm (Source IP, Source Port, Protocol, Dest IP, Dest Port)      ENI                                 ENI (static IP)                         (static IP)
- Connection preservation: Maintains the same TCP connection between client and target (long sessions for gaming or IoT).
- IP preservation: The original client IP address is preserved and visible to the backend Target Group
- Used with AWS VPC PrivateLink to expose services privately Availability Zone 1       Availability Zone 2


## VPC PrivateLink
*(Slide 252)*

```text
                                Region




                                    VPC

                                     Private subnet                                   VPC



                                                                     PrivateLink
                                                      VPC Endpoint                    Network
                                                                                   Load Balancer
                                                                                                   SaaS application
```


## Gateway Load Balancer
*(Slide 253)*


## Gateway Load Balancer (GWLB)
*(Slide 254)*

- It is designed to deploy, scale, and manage third-party virtual appliances like firewalls, intrusion detection/prevention systems (IDS/IPS), and deep packet inspection tools.
- Gateway Load Balancer (GWLB) operates at Layer 3 (IP layer).
- Uses the GENEVE (Generic Network Virtualization Encapsulation) protocol on port 6081 to forward traffic between clients and appliances.
- Transparent to traffic - doesn’t modify IP addresses, so appliances see original             Gateway Load Balancer source and destination IPs.
- Traffic is sent to GWLB endpoints (GWLBe) created in each VPC using AWS PrivateLink.
- GWLB integrates with industry leading partners Aviatrix, Cisco Systems, Fortinet, Palo Alto Networks etc.


## How Gateway Load Balancer work?
*(Slide 255)*


## Use case: Centralized Inspection using GWLB
*(Slide 256)*

```text
                                              VPC



                                                                Appliance or Security VPC
                                                    GWLBe

              VPC                                                          Encap A


                                                                           Encap B
                        GWLBe                               Gateway Load
                                                              Balancer      Encap C   Network Appliances
                                              VPC




                                                    GWLBe
```


## Important to know for your exam
*(Slide 257)*


## Important to know for your exam
*(Slide 258)*

- External and Internal Load balancers
- Cross-zone load balancing TLS certificate
- Server Name Indication (SNI)
- Client IP preservation 80 8080 443            Listeners
- Proxy protocol Target Groups
Target Group 1                 Target Group 2
Targets
EC2        EC2 Instance   Instance Container   Container


## External and Internal Load Balancer
*(Slide 259)*

Availability Zone               Availability Zone VPC External Load Balancer (Public)                     Public subnet                   Public subnet
- To be launched in Public Subnet
- Receives traffic from the internet
Private subnet                 Private subnet
Web Internal Load Balancer (Private)                       EC2               EC2        EC2                EC2 Instance          Instance   Instance           Instance
- To be launched inside the Private subnet                                               Private subnet                 Private subnet
- Receives traffic from within the VPC
Private subnet                 Private subnet
App EC2               EC2               EC2             EC2 Instance          Instance          Instance        Instance


## Cross-zone load balancing
*(Slide 260)*

```text
          Without Cross-Zone load balancing                              Client


                                               Availability Zone                         Availability Zone




                                                                   50%            50%


                                                                          ELB




                                              Application Servers                       Application Servers
```


## Cross-zone load balancing
*(Slide 261)*

```text
          With Cross-Zone load balancing                                 Client


                                               Availability Zone                         Availability Zone




                                                                   33%            66%


                                                                          ELB




                                              Application Servers                       Application Servers
```


## Cross-zone load balancing
*(Slide 262)*

- Enabled by default for Application Load Balancer
- Disabled by default for Network Load Balancer TLS certificate
- Disabled by default for Gateway Load Balancer
80 8080 443            Listeners
Target Groups
Target Group 1                 Target Group 2
Targets
EC2        EC2 Instance   Instance Container   Container


## Server Name Indication (SNI)                                                     Client
*(Slide 263)*

- SNI allows using multiple SSL certificates with the load balancer
- Load balancer can host multiple different websites or         As per client                              www.app1.com web applications                                               requested domain name
- Client to initiate SSL handshake request using desired       use appropriate                             www.app2.com hostname                                                     SSL certificate
- The load balancer will identity the correct certificate                                                  www.app3.com according to the hostname and return the correct certificate
- Only works for ALB & NLB                                     Target            Target     Target Group 1           Group 2    Group 3
www.app1.com    www.app2.com   www.app3.com


## Client IP preservation
*(Slide 264)*

1.2.3.4
- In case of ALB, the target (EC2) sees the ALB’s private IP as the source. Client
- To get the real client IP, use the X-Forwarded-For header in the HTTP request.
- NLB is a pass-through load balancer, so the target sees the client’s original IP directly.
- If you use a TLS listener on NLB (with termination at NLB), the target Load Balancer won’t see the client IP, and in that case, you can enable Proxy Protocol v2 to forward that info.
EC2
…


## NLB Proxy Protocol
*(Slide 265)*

Client
- An Internet protocol used to carry information from the source                       IP: 1.2.3.4 (requesting the connection) to the destination (where connection was requested) SSL
- If the Load Balancer terminates the connection (e.g. TLS termination), the source IP address of the client is not preserved.
- Enable Proxy protocol v2 for passing the connection info to the                  Network Load backend.                                                                           Balancer
- When Proxy protocol v2 is enabled, NLB adds small binary header in the TCP packet which contains
  - Original source IP and port (client)
  - Destination IP and port (load balancer)                      TCP
  - Protocol type (TCP/UDP/TLS)
  - Optional fields for SSL/TLS info
EC2 Instance


## Application Load                   Network Load                          Gateway Load                           Classic Load
*(Slide 266)*

Balancer                          Balancer                              Balancer                               Balancer
- Operates at Layer 7
- Operates at Layer 4
- Operates at Layer 3
- Operates at Layer 4 & 7
- Targets: Instance, IP, Lambda,
- Targets: Instance, IP, ALB
- Targets: Instance, IP
- Targets: Instance, IP, Containers                                                                                                       Lambda, Containers
- Supported protocols TCP, UDP,
- Supported protocols GENEVE
- Supported protocols HTTP, TLS                                                                    •   Supported protocols HTTP, HTTPS, WebSocket, HTTP/2                                                •    Use: Traffic inspection where and gRPC                                                                                                         HTTPS, TCP, SSL
- Provides Static IP so that client      traffic is routed to backed
- Use header X-Forwarded-For for        can whitelist                          Firewall or network Appliance
- Use: Web applications Client Source IP                                                             instances
- Flow-based routing (5-tuple
- Content based routing – Host,         hash) – Long TCP connections path, Query string, SourceIP
- Proxy Protocol v2 for Source IP
- Cross-zone load balancing             when using TLS enabled by default
- Use: Ultra low latency
- Use: Web Applications, API            requirement. Example: Real- hosting time systems, IoT, Gaming, Streaming


## Autoscaling group
*(Slide 267)*


## Auto Scaling group (ASG)
*(Slide 268)*

- Auto Scaling groups automate the scaling of EC2 instances
- You specify Minimum, Maximum, and Desired instance counts
- ASG ensures the desired number is always running.
- Requires a Launch Template to define instance details such as AMI, type, security groups, and key pair.
- Integrates tightly with Elastic Load Balancers (ALB/NLB) and automatically registers/deregisters instances.
- Performs Health Checks (EC2 status and optionally ELB health checks) to replaces unhealthy instances automatically.
- Supports different Scaling policies
- Uses CloudWatch Alarms to trigger scaling actions based on metrics.


## Auto Scaling group scaling policies
*(Slide 269)*

- Manual Scaling
  - Set the required number of EC2 instances in the ASG configuration
- Dynamic Scaling
  - Simple & Step scaling
    - When avg. CPU > 50% add 1 EC2 instance
    - When avg. CPU > 75% add 2 EC2 instances
    - When avg. CPU < 40% remove 1 EC2 instance
    - When SQS queue has high number of messages
  - Target Tracking
    - Maintain the avg. CPU utilization to ~40%
- Scheduled Scaling
  - Schedule the scaling for given period – Saturday evening 6-11pm
- Predictive Scaling
  - User Machine Learning to scale based on the historic utilization


## Demo
*(Slide 270)*


## Auto Scaling Group - Advanced features
*(Slide 271)*

- Termination Policy - Decides which instance to remove first (default: oldest)
- Cooldown Period - Prevents new scaling actions from happening too quickly after a previous one (default: 300 seconds).
- Instance Refresh - Rolling updates of instances to apply new configurations safely. Autoscaling
- Lifecycle Hooks - Allows custom actions (e.g. run scripts) when launching or         Group terminating instances.
- Warm Pools - Let ASG keep pre-initialized instances ready to start faster during scale-out.


## ASG with On-demand and spot instances
*(Slide 272)*

In ASG, define:
- Base Capacity - Minimum number of On-Demand instances.
- On-Demand Percentage Above Base - Percentage of additional instances that should be On-Demand.
- Spot Pools - Groups of instance types that can be used as Spot instances (always diversify).
The ASG will then automatically:
- Launch On-Demand instances first for the base.
- Add Spot instances from multiple pools to reduce interruption risk and replace interrupted Spot instances automatically.
- Use Lifecycle Hooks to handle graceful shutdowns when Spot interruptions happen.


## Let’s talk architecture
*(Slide 273)*


## ELB + Autoscaling Group with Simple scaling
*(Slide 274)*

```text
              VPC
                                        Internet gateway

                                                                                                                         Alarm
                                                         Elastic Load Balancing          Amazon
                                                                  (ELB)
                                                                                       CloudWatch
                                                                                                                              action
                     Auto Scaling Group

                                                                                                        Average CPU >
                                                                                                             80%
                                                                                         Launch EC2
                              EC2               EC2             EC2           EC2
                            Instance          Instance        Instance      Instance                                    Autoscaling
                                                                                                                          Group
                                                         Elastic Load Balancing
                                                                  (ELB)


                     Auto Scaling Group

                                                                                        Terminate EC2   Average CPU <
                                                                                                             30%
                              EC2               EC2             EC2
                            Instance          Instance        Instance
```


## ELB + Autoscaling Group with Target tracking
*(Slide 275)*

```text
              VPC
                                        Internet gateway


                                                         Elastic Load Balancing
                                                                  (ELB)
                                                                                       Target Tracking
                                                                                         CPU ~40%
                     Auto Scaling Group



                                                                                                  Launch / Terminate EC2
                              EC2               EC2             EC2           EC2
                            Instance          Instance        Instance      Instance


                                                                                                                           Autoscaling
                                                         Elastic Load Balancing                                              Group
                                                                  (ELB)

                                                                                       Target Tracking
                     Auto Scaling Group                                                  CPU ~50%

                                                                                                  Launch / Terminate EC2
                              EC2               EC2             EC2
                            Instance          Instance        Instance
```


## Job processing
*(Slide 276)*

```text
             Use case: Dynamically adjust the EC2 capacity depending on the incoming Job requests for data processing

                                                                                          Source                               Target

                                                            1   Upload




                                                                2
                                                            message
                                                                                                Auto scaling Group      5   Processing

                                                                                   poll
                                              Application
                                                                                    4
                                                                    Simple Queue                                        Spot
                                                                       Service
                                                                                            3    Scale based on SQS queue depth

                       Clients
```


## ALB as a target of NLB
*(Slide 277)*

```text
            Use case: Client needs Static IPs to be whitelisted in the corporate network firewall for the web application hosted in
            AWS

                Corporate Network
                                                               IP
                                                               IP


                                                                                HTTPS


                                          Firewall
                                                         Network                      Application
                                                       Load Balancer                 Load Balancer

                        Clients




                                                                                                                              Page 1/2
```


## Blue/Green deployment – ASG + ALB
*(Slide 278)*

```text
             Use case: Blue/green deployment where traffic is gradually shifted to green deployment. Use Weighted target groups
             to shift the traffic gradually. Use separate Autoscaling groups for both target groups and manage the capacity manually
             as traffic shifts.
                                                                                                       Auto scaling Group

                                                                     90%
                                                                               Target
                                                                               Group




                                                   HTTPS


                                                         Application                                    Auto scaling Group
                                                        Load Balancer
                                                                                Target
                                                                                Group
                        Clients                                         10%
```


## 2. ALB as a target of NLB
*(Slide 279)*

```text
             Use case: Client needs Static IPs to be whitelisted in the corporate network firewall for the web application hosted in
             AWS

                Corporate Network
                                                                IP
                                                                IP


                                                                                 HTTPS


                                          Firewall
                                                          Network                      Application
                                                        Load Balancer                 Load Balancer

                        Clients




                                                                                                                              Page 1/2
```


# VPC and Networking

*(Source: Slide 280)*


## We want to a web application in AWS
*(Slide 281)*

```text
                                                                              AWS


                                                               App

                                              Internet

           Users
                                                               DB


                                                         Sample Application
```


## Users
*(Slide 282)*

```text
                                                                                                                                    How to access outbound internet from
                                                                                            Internet
                                                                                                                                    Application servers?


                    Region


                                                                                            Internet Gateway

                                              VPC                                                                     10.0.0.0/16
                                                                                                          2001:db8:1234:1a00::/56

                                                      Availability Zone                              Availability Zone

                                                    Public subnet                                Public subnet


                                                                          Application Load                      NAT Gateway
                                               NAT Gateway                   Balancer

                                                     Private subnet                                 Private subnet




                                                    Private subnet                               Private subnet


                                                          P                   replication                 S
```


## Service VPC
*(Slide 283)*

```text
                                                                                                Users


       Application
                           Network                                                                                       How to access VPC resources privately from employee
                         Load Balancer                                                          Internet
         service                                                                                                         workstation?


                       Region


                                                                                                Internet Gateway

                                                  VPC                                                                     10.0.0.0/16
     Kinesis                                                                                                   2001:db8:1234:1a00::/56
                             PrivateLink                                                                                                  Client VPN
                                                          Availability Zone                              Availability Zone                                        Client
      SQS
                                                        Public subnet                                Public subnet
                                   VPC Endpoint                                                                                                              Corporate data center (on-prem)
      SNS                           (interface)                               Application Load                      NAT Gateway            Direct
                                                   NAT Gateway                   Balancer
                                                                                                                                          Connect
                                                         Private subnet                                 Private subnet


         S3

                                                        Private subnet                               Private subnet
 DynamoDB                                                                                                                          VGW
                                   VPC Endpoint               P                   replication                 S                            VPN
                                                                                                                                                                  192.168.0.0/16
                                    (gateway)
                                                                                                                                                       CGW
```


## VPC Private connectivity options
*(Slide 284)*

- VPC Peering connection
- AWS Transit Gateway
- AWS Cloud WAN
- Amazon VPC Lattice


## Transit Gateway
*(Slide 285)*

```text
           Region
                                                             VPC



                 VPC


                                         Availability Zone

                                 Private subnet

                                                                   VPC



                               Private IP


                                              Application




                                                             VPC
```


## Transit Gateway
*(Slide 286)*

```text
           Region
                                                             VPC



                 VPC


                                         Availability Zone

                                 Private subnet                                    Corporate data center (on-prem)




                               Private IP
                                                                   VPN or Direct
                                                                     Connect
                                                                                            192.168.0.0/16
                                              Application




                                                             VPC
```


## Transit Gateway
*(Slide 287)*

```text
           Region                                                                    Region
                                                             VPC                              VPC



                 VPC


                                         Availability Zone

                                 Private subnet


                                                                   Transit Gateway
                                                                       Peering
                               Private IP


                                              Application




                                                             VPC                          VPC
```


## Service VPC
*(Slide 288)*

```text
                                                                                 Users

                                                                                                                                                                            VPC
                                                                                                                                                                                                DC

                                                                                                                                                                  Transit
                         Network
       Application                                                                                                                                               Gateway
                       Load Balancer                                                                      Cloud WAN              VPC Lattice
         service



                                                                                                                                                                            VPC
                     Region


                                                                                         Internet Gateway
                                                                                                                                                                              VPC
                                                                                                                   10.0.0.0/16
                                                VPC
      Kinesis                                                                                          2001:db8:1234:1a00::/56
                           PrivateLink
                                                        Availability Zone                        Availability Zone
        SQS
                                                      Public subnet                            Public subnet
                                 VPC Endpoint                                                                                                   Peering connection
                                  (interface)                               Application Load
       SNS
                                                 NAT Gateway                   Balancer                    NAT Gateway
                                                                                                                                   Client VPN       Client VPN
                                                       Private subnet                          Private subnet                                                                       Client
                                                                                                                                    endpoint
                                                                                                                                                                              Corporate Data Center
          S3                                                                                                                       VGW                                        (on-premises)

                                                      Private subnet                           Private subnet
  DynamoDB
                                                                                                                                                Direct Connect
                                 VPC Endpoint               P                                      S
                                  (gateway)
                                                                                                                                                                                     192.168.0.0/16
                                                                                                                                                  IPSecVPN
                                                                                                                                                                        CGW
```


## In this section..
*(Slide 289)*

- VPC and it’s components
- Network Monitoring
- Subnets, Route tables, Internet gateway, NAT
- VPC Flow Logs gateways                                          • VPC Traffic Mirroring
  - Security group and Network ACL
- VPC endpoints and PrivateLink
- IPv6
- IPv6 in VPC and Dual-stack
- VPC Private Connectivity Options
- Egress-only Internet Gateway
  - VPC Peering
  - Transit Gateway
- Hybrid Networking
  - Site-to-Site VPN
  - Direct Connect


## Amazon VPC
*(Slide 290)*


## Web
*(Slide 291)*

```text
                                                         Users                             Browser
                                                                                                                           CloudFront
           myapp.com on AWS
                                                    Route53              myapp.com                                                             Edge
                                                                                                                                             Locations


                                              Virtual private cloud (VPC)
                                                                      ELB
                                                                                  Auto
                                                                                 Scaling                                     Lambda

                                        Web                                                                                  Video
                                       Server            EC2 E        EC2 E
                                                                 B           B                                               Convert
                                                                 S           S                Rekognition             S3                S3
                 SNS
                                                                                                        AI models                                    QuickSight
                                                                                                        enhancement

                                        App
                                                         EC2 EB       EC2 E
                                       Server                                B
                 SES                                             S           S

                                                                                                  Deploy custom
                                                                                                      model
                                                                                                                  Sagemaker


                SQS
                                     ElastiCache

                                                                            Neptune        Kinesis                    S3              EMR
                                                                                                                                                         Redshift
           CloudWatch                         Multi-AZ




                                                          RDS        DynamoDB              Glue
```


## Router
*(Slide 292)*

```text
                                                           Switch
                                              Router

                          LAN A
         Switch                                  Switch   LAN B
                                                                    VPC


           Switch                               Switch
```


## Traditional IT network vs VPC
*(Slide 293)*

```text
                                                                         AWS VPC
                                Physical Network


                                  Router

                                                              Subnet A       Subnet B




                                                               EC2            EC2


                              Hub or Switch   Hub or Switch



                                                               AZ1            AZ2
```


## Amazon Virtual Private Cloud (VPC)
*(Slide 294)*

- A logically isolated virtual network in the cloud which closely resembles the traditional IT network
- VPC is assigned a Private IP Address range (called CIDR) e.g. 10.0.0.0/16 (IPv4)
- VPC can have both IPv4 (32 bit) or IPv6 (128 bit) IP addresses
VPC building blocks:                                     Region
  - VPC CIDR                                          VPC                                     10.0.0.0/16
  - Subnets and Route table                                 Availability Zone     Availability Zone
Subnet
  - IP Addresses – IPv4 and IPv6                                                Subnet
  - Internet Gateway
  - NAT Gateway
  - VPC Firewalls - Security Group and Network ACL


## VPC Addressing - CIDR
*(Slide 295)*


## CIDR – Classless Inter Domain Routing
*(Slide 296)*

- IP addressing scheme that replaces old address style of Class A, B, C
- Represented as an IP address and prefix
  - Example: IPv4 CIDR 192.168.0.0/16 Prefix
192            .168              .0               .0              /16 1 1 0 0 0 0 0 0 . 1 0 1 0 1 0 0 0. 0 0 0 0 0 0 0 0 .0 0 0 0 0 0 0 0 /16 16 bits                           16 bits
Network Address                  Host Addresses
192.168                       0-255.0-255 Fixed                     For host addresses


## CIDR – Classless Inter Domain Routing
*(Slide 297)*

Formula to calculate number of available IP addresses inside the VPC or Subnets
- Total addresses = 2 ^ (total bits – prefix) Examples:
- 192.168.0.0/16 => 2 ^ (32 – 16) = 2 ^ 16 = 65536
- 192.168.0.0/24 => 2 ^ (32 – 24) = 2 ^ 8 = 256
192            .168            .0              .0
1 1 0 0 0 0 0 0 . 1 0 1 0 1 0 0 0. 0 0 0 0 0 0 0 0 .0 0 0 0 0 0 0 0 /16
192            .168            .0              .0
1 1 0 0 0 0 0 0 . 1 0 1 0 1 0 0 0. 0 0 0 0 0 0 0 0 .0 0 0 0 0 0 0 0 /24


## CIDR – Classless Inter Domain Routing
*(Slide 298)*

```text
                                                                                              192.168.0.0/16
                       VPC
                                              Total IP addresses = 256 x 256 = 65536

                              192.168.0.0          192.168.1.0           192.168.2.0         192.168.255.0

                              192.168.0.1          192.168.1.1           192.168.2.1         192.168.255.1

                              192.168.0.2          192.168.1.2           192.168.2.2         192.168.255.2

      256                                                                                …
                              192.168.0.3          192.168.1.3           192.168.2.3         192.168.255.3

                              192.168.0.4          192.168.1.4           192.168.2.4         192.168.255.4


                              …                    …                     …                   …


                              192.168.0.255        192.168.1.255         192.168.2.255       192.168.255.255


                                                                   256
```


## VPC Addressing
*(Slide 299)*

- AWS VPC CIDR (IPv4)
  - VPC prefix between /16 (65536 IPs) and /28 (16 IPs)
  - RFC 1918 IP ranges for Private network and corresponding AWS recommended ranges
    - 10.0.0.0/8 => 10.0.0.0 – 10.255.255.255   => AWS CIDR 10.X.0.0/16
    - 172.16.0.0/12 => 172.16.0.0 - 172.31.255.255      => AWS CIDR 172.16.0.0/16 to 172.31.0.0/16
    - 192.168.0.0/16 => 192.168.0.0 - 192.168.255.255      => AWS CIDR 192.168.0.0/16
- AWS VPC CIDR (IPv6)
  - VPC CIDR with prefix /56 (2^72 IPs)
  - IPv6 CIDR is allocated by AWS
  - IPv6 IP addresses are globally unique and publicly routable


## More about CIDR.. Watch this youtube video
*(Slide 300)*

*https://youtu.be/O3fgul-fJCk*


## Exercise – Create a VPC
*(Slide 301)*

- Create a VPC with CIDR Region 10.10.0.0/16
VPC   10.10.0.0/16


## Subnets, Route tables & Internet gateway
*(Slide 302)*


## VPC Subnets
*(Slide 303)*

- VPC network is partitioned into smaller networks called     Region Subnets Availability Zone   Availability Zone   Availability Zone Main
- Subnets are created inside a specific Availability Zone VPC                                        10.10.0.0/16
- VPC has a local router which routes traffic within the Subnet A             Subnet B            Subnet C VPC
- Route tables defines the routing logic for the subnets
- VPC has a main (default) route table. All subnets follow main route table by default. 10.10.1.0/24         10.10.2.0/24        10.10.3.0/24 Destination               Target 10.10.0.0/16               Local
- We can create subnet specific route tables.                                     Local Router


## Internet Gateway
*(Slide 304)*

- Internet gateway connects VPC to the                     Region internet Availability Zone   Availability Zone   Availability Zone
- If subnet route table has route to internet via            VPC                                       10.10.0.0/16 the internet gateway, it’s called Public Public subnet        Private subnet subnet Destination                Target 10.10.0.0/16                 Local 0.0.0.0/0        Internet Gateway (igw-x)
10.10.0.0/24         10.10.1.0/24
- If subnet route table does not have route to internet, it’s called a Private subnet Destination                 Target 10.10.0.0/16                Local


## Exercise – Add Internet gateway, Public/Private subnets
*(Slide 305)*

- Create a VPC with CIDR Region 10.10.0.0/16 Availability Zone   Availability Zone                    •   Create an Internet Gateway 10.10.0.0/16                and associate with the VPC VPC
- Create 2 subnets in 2 different availability zones
- Create a route table, add route Private subnet Public subnet                                               entry for internet and associate with first subnet (Public Destination                           Target                                                           subnet) Destination           Target
- Create another route table and 10.10.0.0/16                          Local                        10.10.0.0/16           Local        associate with another subnet 0.0.0.0/0                         igw-xxxxx                                                          (Private subnet)
10.10.0.0/24        10.10.1.0/24


## Exercise – Launch EC2 instances
*(Slide 306)*

- Launch EC2-A in Public subnet Region (assign Public IP)
- Launch EC2-B in the Private subnet 10.10.0.0/16       (no Public IP) VPC
- Connect to EC2-A over SSH from your workstation. Public subnet
- From EC2-A, SSH into EC2-B (for this you will need ssh private key on EC2-A                            EC2-A instance) Public IP 10.10.0.0/24
Private subnet
EC2-B Private IP 10.10.1.0/24


## Exercise – Launch EC2 instances
*(Slide 307)*

- Launch EC2-A in Public subnet Region (assign Public IP)
- Launch EC2-B in the Private subnet 10.10.0.0/16            (no Public IP) VPC
- Connect to EC2-A over SSH from your workstation. Public subnet
- From EC2-A, SSH into EC2-B (for this you will need ssh private key on EC2-A                                               EC2-A instance) Public IP
- After logged into EC2-B, try to ping 10.10.0.0/24             to google.com
Private subnet Does it work? $ping google.com EC2-B                                      •       EC2-B is not in a Public subnet Private IP                                 •       EC2-B does not have a Public IP 10.10.1.0/24


## NAT Gateway
*(Slide 308)*


## NAT Gateway
*(Slide 309)*

Internet
- NAT Gateway allows instances in a private            Region
subnet to connect to services outside of the VPC, but external services cannot initiate a connection      VPC                                                    10.10.0.0/16 with those instances.
- It’s AWS managed providing higher bandwidth,                                     Availability Zone
better availability, no administration                        Public subnet
- Pay by the hour for usage and bandwidth
- 5 Gbps of bandwidth with automatic scaling up to Web Server 100 Gbps                                                                                             NAT Gateway
- No security groups Private subnet
- Supported protocols: TCP, UDP, and ICMP
- For outbound internet access, NAT Gateway should be created in Public Subnet and should                       App Server be allocated an Elastic IP


## NAT Gateway High availability
*(Slide 310)*

Internet
- NAT Gateways are highly available within a   Region
single AZ
- For HA across multiple AZs, multiple NAT        VPC                                                          10.0.0.0/16 gateways can be launched Availability Zone                      Availability Zone
Public subnet    10.0.0.0/24
NAT Gateway
Private subnet   10.0.1.0/24         Private subnet     10.0.2.0/24
App Server                            App Server


## NAT Gateway High availability
*(Slide 311)*

Internet
- NAT Gateways are highly available within a      Region
single AZ
- For HA across multiple AZs, multiple NAT           VPC                                                            10.0.0.0/16 gateways can be launched Availability Zone                       Availability Zone
Public subnet    10.0.0.0/24        Public subnet
NAT Gateway                             NAT Gateway
Private subnet   10.0.1.0/24          Private subnet      10.0.2.0/24 AWS has launched Regional NAT gateway during Re:Invent 2025 App Server                              App Server


## Exercise - NAT Gateway
*(Slide 312)*

Internet
- Launch EC2-A in Public subnet (assign Region Public IP)
- Launch EC2-B in the Private subnet (no 10.10.0.0/16 Public IP) VPC
- Connect to EC2-A over SSH from your Availability Zone workstation.
- From EC2-A Public subnet
- Create another Public subnet C
- Create NAT gateway in this new subnet.
- Update Private subnet route table and Web Server                                                      add entry for the destination 0.0.0.0/0 NAT Gateway with target as NAT gateway. Private subnet                                                    •   Check the outbound internet connectivity from EC2-B
App Server


## Regional NAT Gateway (New)
*(Slide 313)*

Internet
- Regional NAT gateway (RNAT) works at VPC          Region
level and spans across multiple Availability Zones                                                Virtual private cloud (VPC)                           10.10.0.0/16
- Automatically expands into newer AZs as you                                                     Availability Zone Availability Zone deploy your workloads Destination                   Target
- Optionally can choose a manual mode to                             Regional 10.10.0.0/16                  Local NAT Gateway configure AZs for the RNAT                                                                0.0.0.0/0             IGW-xxxxxx
- Maintains Zonal affinity (Saves inter-AZ Data transfer charge)                                           Private subnet                     Private subnet
- No need to have Public subnets for the regional NAT gateway                                                  App Server                         App Server
- Comes with its own Route Table                                     Destination                Target
10.10.0.0/16                Local
0.0.0.0/0      Nat-regional- xxxxxx


## Regional NAT Gateway
*(Slide 314)*

```text
                                                                                           Internet



                                              Mumbai
                                                       Virtual private cloud (VPC)                                10.10.0.0/16

                                                               Availability Zone                      Availability Zone            Destination       Target

                                                                                                                                   10.10.0.0/16       Local
                                                                      Regional
                                                                    NAT Gateway                                                      0.0.0.0/0    IGW-xxxxxx




                                                            Private subnet A                     Private subnet B
                                                                                                                                   Destination          Target

                                                                                                                                   10.10.0.0/16          Local

                                                               App Server                             App Server                    0.0.0.0/0     Nat-regional- xxxxxx
                                                                            10.10.1.0/24                            10.10.2.0/24
```


## Security group and Network ACL
*(Slide 315)*


## Firewalls in VPC
*(Slide 316)*

- Security Groups
- Network Access Control List (NACL)
VPC
Subnet
N   Inbound Traffic Security   A EC2 Group     C L   Outbound Traffic


## Security Group - Recap
*(Slide 317)*

- Security Groups are most basic, native and important firewall for EC2 instances
- Security group has Inbound and Outbound rules
- Security group has only ALLOW rules. Does not support DENY/Block rules.
- Default Security group in each VPC
- Authorises traffic for both IPv4 and IPv6 traffic
- Security groups are stateful – return traffic is automatically allowed
ssh     Inbound Rules 22
http 80 Outbound Rules tcp xxxx
IP 5.6.7.8 Security Group EC2


## Default Security Group
*(Slide 318)*


## Network ACLs
*(Slide 319)*


## Network Access Control List (NACL)
*(Slide 320)*

- Works at Subnet level – Hence automatically applied to all instances                     Subnet A - 10.0.0.0/24
- Contains both Allow and Deny rules. Rules are numbered.                               Subnet
- Rules are evaluated in the order of rule number (1 to 32766)                          EC2           sg sg           N A
- Stateless – We need to explicitly open ports for return traffic                                                   C sg
- Default NACL allows all inbound and outbound traffic                                  EC2                         L
- NACL are a great way of blocking a specific IP at the subnet level Network ACL inbound rules #Rule                   Type        Protocol   Port              Source                  Allow/Deny
100             All IPv4 traffic     All      All          180.151.138.43/32               DENY
101                  HTTPS           TCP      443              0.0.0.0/0                  ALLOW
*             All IPv4 traffic     All      All              0.0.0.0/0                   DENY


## Network ACL – Allow return traffic                            Rule         Protocol
*(Slide 321)*

```text
                                                                                         Inbound Rules
                                                                                               Port          Source           Allow/
                                                                number                                                        Deny
                          Subnet                                  100           TCP             80          1.2.3.4/32         Allow

                                                                  200           TCP             80        9.10.11.12/32        Allow
                                               Inbound Rules
                                   http                               *    All IPv4 traffic     All         0.0.0.0/0          Deny
                                    80
                                                                                                      9.10.11.12
                                                                                                      (port: XXXX)
                                   ssh
                                   22

                                               Outbound Rules


                                     tcp
                                    xxxx
                                                                                       Outbound Rules
                                                                 Rule       Protocol          Port       Destination         Allow/
                      5.6.7.8                                   number                                                       Deny

                       EC2                    Network ACL        100           TCP            XXXX        1.2.3.4/32          Allow

                                                                 200           TCP            XXXX       9.10.11.12/32        Allow

                                                                  *       All IPv4 traffic     All         0.0.0.0/0          Deny
```


## Default Network ACL
*(Slide 322)*

```text
                                                                                           Inbound Rules
                                                                   Rule           Protocol         Port      Source           Allow/
                          Subnet
                                                                  number                                                      Deny

                                               Inbound Rules          100      All IPv4 traffic     All      0.0.0.0/0         Allow
                                   http                                *       All IPv4 traffic     All      0.0.0.0/0         Deny
                                    80



                                   ssh
                                   22

                                               Outbound Rules


                                     tcp
                                    xxxx
                                                                                          Outbound Rules
                                                                 Rule         Protocol            Port     Destination        Allow/
                      5.6.7.8                                   number                                                        Deny

                       EC2                    Network ACL        100        All IPv4 traffic      All       0.0.0.0/0          Allow

                                                                  *         All IPv4 traffic      All       0.0.0.0/0          Deny
```


## Network Access Control List (NACL)
*(Slide 323)*

- Works at Subnet level – Hence automatically applied to all instances Subnet A - 10.0.0.0/24
- Contains both Allow and Deny rules. Rules are numbered. Subnet
- Rules are evaluated in the order of rule number (1 to 32766) EC2           sg sg           N
- Stateless – We need to explicitly open ports for return traffic                                                        A
- Default NACL allows all inbound and outbound traffic                                                                   C sg EC2                         L
- NACL are a great way of blocking a specific IP at the subnet level
Network ACL inbound rules #Rule                   Type        Protocol          Port              Source                Allow/Deny
100             All IPv4 traffic     All              All       180.151.138.43/32               DENY
101                  HTTPS           TCP             443               0.0.0.0/0               ALLOW
*             All IPv4 traffic     All              All              0.0.0.0/0                DENY


## Default Network ACL (IPv4 & IPv6)                                                 Inbound Rules
*(Slide 324)*

```text
                                                                  Rule        Protocol         Port        Source           Allow/
                          Subnet                                 number                                                     Deny

                                                                   100      All IPv4 traffic    All        0.0.0.0/0         Allow
                                               Inbound Rules
                                                                   101      All IPv6 traffic    All          ::/0            Allow
                                   http
                                    80                                *     All IPv4 traffic    All        0.0.0.0/0         Deny

                                                                      *     All IPv6 traffic    All          ::/0            Deny
                                   ssh
                                   22

                                               Outbound Rules


                                     tcp
                                    xxxx                                               Outbound Rules
                                                                 Rule       Protocol           Port       Destination        Allow/
                                                                number                                                       Deny
                      5.6.7.8
                                                                 100      All IPv4 traffic      All        0.0.0.0/0          Allow
                       EC2                    Network ACL        101      All IPv6 traffic      All           ::/0            Allow

                                                                  *       All IPv4 traffic      All        0.0.0.0/0          Deny

                                                                  *       All IPv6 traffic      All           ::/0            Deny
```


## Security Groups vs Network ACL
*(Slide 325)*

```text
                                  Security Group                       Network ACL

               Operates at EC2 instance                  Operates at Subnet level


               Supports only Allow rules                 Supports both Allow and Deny rules


               Stateful – Return traffic is allowed      Stateless – Return traffic needs to be authorized
                                                         in Outbound rules

                                                         Rules are evaluated in the order (lower to
               All rules are evaluated before making a
                                                         higher) and first matching rule is applied
               decision
```


## VPC Endpoints and AWS PrivateLink
*(Slide 326)*


## Without VPC endpoints and PrivateLink
*(Slide 327)*

```text
                                Region

                                                         Internet Gateway   1
                                                                                 S3         DynamoDB
                                    VPC

                                     Public subnet                                    VPC

                                                          $
                                                                            2
                                                NAT Gateway
                                                                                     Network
                                                                                   Load Balancer
                                       Private subnet                                                           SaaS application


                                                                                                                          100+
                                                                                                                          services
                                                                                API Gateway
                                                                                              SageMaker   KMS     SQS
                                                                            3

                                                 Private IP
                                                                                  Kinesis     EC2 API     S3    DynamoDB
```


## With VPC endpoints and PrivateLink
*(Slide 328)*

```text
                                Region
                                                                                                      1
                                                          Internet Gateway
                                                                                                           S3         DynamoDB
                                    VPC

                                     Public subnet                                                              VPC

                                                                                                      2
                                                NAT Gateway
                                                                                                               Network
                                                                               Free                          Load Balancer
                                       Private subnet                                                                                   SaaS application
                                                                    Gateway Endpoint

                                                                                                                                                  100+
                                                                                                                                                  services
                                                                                $                         API Gateway SageMaker
                                                                                                                                  KMS     SQS
                                                                                                      3

                                                                                        PrivateLink
                                                 Private IP    ENI Interface endpoint

                                                                                                            Kinesis     EC2 API   S3    DynamoDB
```


## VPC Endpoints and AWS PrivateLink
*(Slide 329)*

- VPC Endpoints allow you to connect to resources in another VPCs and AWS Services using a private network instead of the public network
- They remove the need of IGW, NAT GW to access AWS Services
- Endpoint devices are horizontally scaled, redundant and highly available without any bandwidth constraint on your network traffic
- Gateway Endpoint: To access Amazon S3 and DynamoDB only
- Interface Endpoint: To access broad set of other AWS services (e.g. SQS, SNS, Kinesis, S3 etc.) and customer services deployed in other VPCs and across AWS accounts
- Other types of Endpoints: Gateway Load Balancer endpoint, Resource endpoint, VPC lattice Service- network endpoint


## VPC Gateway Endpoint
*(Slide 330)*


## VPC Gateway Endpoint
*(Slide 331)*

```text
                                Region



                                                                                            S3         DynamoDB
                                    VPC

                                                                                                 VPC
                                       Private subnet




                                                                                                Network
                                                                                              Load Balancer
                                                                                                                            SaaS applications
                                                        Gateway Endpoint




                                  Client                                                   API Gateway
                                                                                                         SageMaker   KMS    SQS

                                                        Interface endpoint   PrivateLink                                             100+
                                                                                                                                     services
                                                                                             Kinesis     EC2 API     S3    DynamoDB
```


## VPC Gateway Endpoint
*(Slide 332)*

- Enables private connection between VPC and S3/DynamoDB
- Need to modify the route tables and add an entry to route the traffic to S3 or DynamoDB through the gateway VPC endpoint
- When we create Gateway endpoint, a prefix list is created in VPC
- The prefix list is the collection of IP addresses for AWS services such as Amazon S3 or DynamoDB.
- The Prefix list is formatted as pl-xxxxxxxx and becomes an available option in both subnet routing tables and security groups


## VPC Gateway Endpoint
*(Slide 333)*

- Prefix list should be added in Security group Outbound rule (if Security group outbound rules do not have default “Allow All” rule)
  - VPC Gateway endpoint can only be accessed from within the VPC in which it’s created in the same AWS region


## Can be accessed only from within the VPC
*(Slide 334)*

```text
                                                Region

                                                                   VPC

              Corporate
                                                                    Private subnet
              data center


                                      VPN/DX
                                                                   X                 Gateway
                                                                                     Endpoint   S3




                                               VPC

                                                         Peering

                                                                   X
```


## VPC Gateway Endpoint
*(Slide 335)*

- Prefix list should be added in Security group Outbound rule (if Security group outbound rules do not have default “Allow All” rule)
  - VPC Gateway endpoint can only be accessed from within the VPC in which it’s created in the same AWS region
  - It’s free to use and hence always recommended to use VPC Gateway endpoint when you want to access Amazon S3 or DynamoDB from applications deployed in the VPC


## VPC Gateway Endpoint - Demo
*(Slide 336)*


## VPC Endpoint for S3 - Demo
*(Slide 337)*

```text
                                Region
                                                                                                      Private subnet route table
                                    VPC                               VPC A 10.10.0.0/16
                                       Public subnet              Private subnet                Destination          Target

                                                                                                10.10.0.0/16         local

                                                                                                pl-xxxxxxx           vpce-xxxxxxxx

                    ssh                                     ssh


                                                Public IP             Private IP
                                                (bastion)


                                         10.10.0.0/24             10.10.1.0/24
                                                                                           Gateway
                                                                                           Endpoint                          S3
```


## Demo steps
*(Slide 338)*

1. Create VPC with two subnets – A public and a private subnet
2. Launch an EC2 instance (EC2-A) (bastion host) in the Public subnet (allow SSH for 0.0.0.0/0)
3. Launch an EC2 instance (EC2-B) in the Private subnet (allow SSH from SG of bastion host)
4. Create IAM role for EC2 instance to allow S3Full permissions. Attach it to EC2-B.
5. Login to EC2-A over SSH and from there SSH into EC2-B (you will need .pem key file on EC2-A)
6. Try accessing S3 bucket using
7. Create VPC Gateway endpoint for S3 - Select Private subnet
8. Modify Private subnet route table to route traffic to S3 via VPC gateway endpoint
9. Test the connectivity to S3 by uploading/downloading some files from any of your S3 bucket aws s3 ls
aws s3 cp s3://bucket_name/file_name .


## VPC Interface endpoint
*(Slide 339)*

*Powered by AWS PrivateLink*


## VPC Interface Endpoint
*(Slide 340)*

```text
                                Region



                                                                                            S3         DynamoDB
                                    VPC

                                                                                                 VPC
                                       Private subnet




                                                                                                Network
                                                                                              Load Balancer
                                                                                                                          SaaS applications
                                                        Gateway Endpoint
                                                                                                                                       100+
                                                                                                                                       services
                                  Client                                                   API Gateway
                                                                                                         SageMaker   KMS        SQS

                                                        Interface endpoint   PrivateLink


                                                                                             Kinesis      EC2 API    S3      DynamoDB
```


## VPC Interface endpoint
*(Slide 341)*

- Interface endpoints create local IP addresses (using ENI) in your VPC.
- Has Security Group – inbound rules to be configured
- For High Availability create VPC endpoints across multiple Availability zones
- Supports IPv4 and IPv6 traffic.
- Supports traffic over TCP and UDP protocols
- Interface endpoint gets a regional and zonal DNS
  - Regional: vpce-0b7d2995e9dfe5418-mwrthsyu.sqs.us-east-1.vpce.amazonaws.com
  - Zonal: vpce-0b7d2995e9dfe5418-mwrthsyu-us-east-1a.sqs.us-east-1.vpce.amazonaws.com


## VPC endpoint - Security group
*(Slide 342)*

```text
                                                                                                                              Inbound Rules
                                                                                                                Protocol            Port         Source

                                                   Consumer VPC (10.10.0.0/16)                                    TCP               443       10.10.1.0/16



                                                     Private subnet (10.10.1.0/24)
                               Availability Zone




                                                                                       Security group

                                                                                                        https

                                                                                     10.10.1.24
                                                                                                                PrivateLink           SQS
                                                   Client                             ENI
```


## VPC Interface endpoint – Important to know
*(Slide 343)*

- You can expose your own services using VPC endpoint service
- The Service provider VPC and consumer VPC can have overlapping CIDR blocks
- Interface endpoint can be accessed from other networks e.g. Peered VPCs, Transit gateway, VPN or Direct Connect


## VPC Endpoint Service
*(Slide 344)*

```text
                                Region



                                                                                            S3         DynamoDB
                                    VPC

                                                                                                 VPC
                                       Private subnet




                                                                                                Network
                                                                                              Load Balancer
                                                                                                                           SaaS applications
                                                        Gateway Endpoint
                                                                                                                                     100+
                                                                                                                                     services
                                  Client                                                   API Gateway
                                                                                                         SageMaker   KMS      SQS

                                                        Interface endpoint   PrivateLink


                                                                                             Kinesis      EC2 API    S3    DynamoDB
```


## VPC endpoint service
*(Slide 345)*

```text
                                                             10.10.0.0/16                  VPC Endpoint Service
                                              Consumer VPC
                                                                                           Provider VPC 10.10.0.0/16




                                       Client                    Interface   PrivateLink     Network
                                                                 Endpoint                  Load Balancer
```


## Accessing Interface endpoint from remote network
*(Slide 346)*

```text
                                                                                  Consumer VPC (10.10.0.0/16)



                                                                                    Private subnet (10.10.1.0/24)
                    On-premises




                                                              Availability Zone
                                                 VPN or                                                             10.10.1.24
                                                                                                                                 Interface   PrivateLink
                                              DirectConnect                                                                      Endpoint                    S3
                                                                                                                     ENI
```


## VPC Interface endpoint - DNS
*(Slide 347)*


## Interface endpoint DNS
*(Slide 348)*

- Interface endpoint gets a regional and zonal DNS
    - Regional: vpce-0b7d2995e9dfe5418-mwrthsyu.sqs.us-east-1.vpce.amazonaws.com
    - Zonal: vpce-0b7d2995e9dfe5418-mwrthsyu-us-east-1a.sqs.us-east-1.vpce.amazonaws.com
- Private DNS settings for VPC interface endpoint
  - VPC Setting: “Enable DNS hostnames” and “Enable DNS Support” must be 'true’
  - The public hostname of an AWS service will resolve to the private Interface endpoint hostname
- With Private DNS enabled, the consumer VPC can access the endpoint services using Service’s default DNS e.g sqs.us-east-1.amazonaws.com instead of using endpoint specific DNS e.g vpce-12345-ab.ec2.us-east- 1.vpce.amazonaws.com


## VPC interface endpoint DNS
*(Slide 349)*

```text
                                                   Consumer VPC (10.10.0.0/16)


                                                                                                         vpce-0b7d2995e9dfe5418-mwrths3x-us-east-1a.sqs.us-east-1.vpce.amazonaws.com
                                                    Private subnet (10.10.1.0/24)
                               Availability Zone




                                                                           10.10.1.24   Interface endpoint
                                                                                                                PrivateLink               SQS
                                                   Client



                                      aws sqs send-message --queue-url https://sqs.ap-south-
                                      1.amazonaws.com/<account id>/myqueue --message-body "Hello from AWS"
                                      -–endpoint-url https://vpce-0b7d2995e9dfe5418-mwrths3x-us-east-
                                      1a.sqs.us-east-1.vpce.amazonaws.com
```


## VPC interface endpoint DNS
*(Slide 350)*

```text
                                                          Enable DNS Support
                                                          Enable DNS hostnames
                                                                                                   sqs.us-east-1.amazonaws.com         SQS endpoint PUBLIC IP
                                                    Consumer VPC (10.10.0.0/16)


                                                                                                       vpce-0b7d2995e9dfe5418-mwrths3x-us-east-1a.sqs.us-east-1.vpce.amazonaws.com
                                                     Private subnet (10.10.1.0/24)
                               Availability Zone




                                                                          10.10.1.24   Interface endpoint
                                                                                                             PrivateLink               SQS
                                                    Client




                                             aws sqs send-message --queue-url https://sqs.ap-south-1.amazonaws.com/<account
                                             id>/myqueue --message-body "Hello from AWS"
```


## VPC Interface Endpoint - Demo
*(Slide 351)*


## VPC Interface Endpoint - Demo
*(Slide 352)*

```text
                                Region

                                    VPC                               VPC A 10.10.0.0/16
                                       Public subnet              Private subnet




                                                                                            Security Group

                    ssh                                     ssh                                                   HTTPS

                                                                                                      Interface
                                                                      Private IP                      Endpoint            SQS
                                                Public IP
                                                (bastion)


                                         10.10.0.0/24             10.10.1.0/24
                                                                                           Gateway
                                                                                           Endpoint                       S3
```


## Demo steps (using existing setup used for gateway endpoint demo)
*(Slide 353)*

1. Create SQS queue in the same AWS region (say myqueue)
2. Enable “Enable DNS hostnames” and “Enable DNS Support” for the VPC
3. Create Security group for VPC interface endpoint ENI – Inbound HTTPS (443) from EC2-B instance SG
4. Create or modify IAM role for EC2 to allow Amazon SQS permissions and attach it to EC2-B
5. Login (SSH) into EC2-A and from there SSH into EC2-B and try accessing SQS service using CLI (command below. No connectivity)
6. Create VPC interface endpoint for SQS - Select Private Subnet and Enable Private DNS
7. From EC2-B, now test the connectivity to SQS by sending some messages
aws sqs send-message --queue-url https://sqs.ap-south- 1.amazonaws.com/<account id>/myqueue --message-body "Hello from AWS nerd"


## VPC endpoints security
*(Slide 354)*


## VPC endpoint security – Network layer
*(Slide 355)*

```text
                                Region



                                                                                                            S3         DynamoDB
                                    VPC

                                                                                                                 VPC
                                       Private subnet




                                                                                                                Network
                                  EC2 SG –
                                                                                                              Load Balancer
                                Outbound Rule                                                                                            SaaS application
                                                                        Gateway Endpoint

                                                                                                                                                    100+
                                                        Endpoint SG –
                                                         Inbound Rule                                                                               services
                                  Client                                                                   API Gateway SageMaker
                                                                                                                                   KMS       SQS

                                                                        Interface endpoint   PrivateLink


                                                                                                             Kinesis     EC2 API   S3      DynamoDB
```


## IAM policies
*(Slide 356)*

*IAM User/Role*

*Identity-based policy   VPC endpoint Policy   Resource-based Policy*


## VPC endpoint security – Endpoint policies
*(Slide 357)*

```text
                                Region



                                                                                                          S3      DynamoDB
                                    VPC


                                       Private subnetEndpoint Policy
                                                                                                               VPC



                                                                                                             Network
                                                                                                           Load Balancer
                                                                                                                                         SaaS application
                                                                       Gateway Endpoint

                                                                                                                                                      100+
                                                                                                                                                      services
                                  Client                                                                  API Gateway
                                                                                                                        SageMaker   KMS        SQS

                                                                       Interface endpoint   PrivateLink


                                                                                                            Kinesis      EC2 API    S3       DynamoDB
```


## Default VPC Endpoint Policy
*(Slide 358)*

- Default endpoint policy grants full access to the endpoint


## VPC Endpoint Policy
*(Slide 359)*

- VPC Endpoint Policy to restrict access to a specific AWS service resource
Restrict access to                      Restrict access to specific S3 Bucket                      DynamoDB table


## VPC Endpoint Policy
*(Slide 360)*

- VPC Endpoint Policy to restrict access for a specific IAM account/user/role
Restrict access for                       Restrict access for specific AWS account                      specific IAM Role


## Using IAM Resource based policies
*(Slide 361)*

```text
                                Region



                                                                                           S3         DynamoDB
                                    VPC


                                       Private subnet
                                                                                                  VPC



                                                                                              Network
                                                                                            Load Balancer
                                                                                                                           SaaS application
                                                        Gateway Endpoint

                                                                                                                                        100+
                                                                                                                                        services
                                  Client                                                   API Gateway
                                                                                                          SageMaker   KMS        SQS

                                                        Interface endpoint   PrivateLink


                                                                                                Kinesis    EC2 API    S3       DynamoDB
```


## AWS Resource based policy - S3 bucket policy
*(Slide 362)*

- S3 bucket policy may have
  - Condition: "aws:sourceVpce": "vpce-1a2b3c4d" -> Allow or Deny from a specific VPC endpoint
  - Condition: "aws:sourceVpc": "vpc-111bbb22" -> Allow or Deny access from a specific VPC


## VPC Private Connectivity options
*(Slide 363)*


## VPC connectivity options
*(Slide 364)*

- VPC Peering connection
- AWS Transit Gateway
- AWS Site-2-Site VPN
- AWS Client VPN
- AWS Direct Connect


## Region B
*(Slide 365)*

```text
                                                            VPC D



                                                                                                                 VPC Peering




                    Region A

                      VPC A                                                                              VPC B


                               Availability Zone                       Availability Zone

                            Public subnet                             Public subnet
                                                                                           VPC Peering
                                                   Application Load
                               NAT Gateway
                                                      Balancer           NAT Gateway

                               Private subnet                          Private subnet
                                                                                                         VPC C


                              Private subnet                          Private subnet


                                   P                                      S
                                                                                           VPC Peering
```


## VPC Peering
*(Slide 366)*

- Simplest way to connect two VPCs privately                                Region A
- It’s One-to-One connection                                            VPC A 10.0.0.0/16
- No need to have internet gateways
- VPCs should not have overlapping CIDRs
- Support intra region and inter region peering
- Need to add route for routing the traffic through the peering connection Region B
B-C
VPC B        VPC Peering VPC C 10.0.0.0/16


## VPC Peering
*(Slide 367)*

```text
                                                      Region                           Mumbai Region                 Region                    N.Virginia Region



                                                                                  10.100.0.0/16                                                10.200.0.0/16
                                                              VPC-A                                                       VPC-B

                                                                 Availability Zone 1                                          Availability Zone 1
                            Destination          Target
                                                                                                                                                    Destination        Target
                           10.100.0.0/16      Local
                                                                                                                                                    10.200.0.0/16   Local
                                                                Private subnet                                                Private subnet
                           10.200.0.0/16      pcx-xxxxx
                                                                                                                                                    10.100.0.0/16   pcx-xxxxx

                                                          Private IP
                                                                                                         X            Private IP

                                                                                                                                   EC2-C
                                                                       EC2-B
                                                                10.100.11.0/24                                                10.200.11.0/24
                                                                                                       VPC Peering




                                                                  ap-south-1a                                                       us-east-1a
```


## Region B
*(Slide 368)*

```text
                                                              VPC


                                                                                                            What if there are tens of VPCs
                                                                                                            and need to communicate with
                                                                                                            each other privately?


                    Region A

                      VPC                                                                                   VPC


                                 Availability Zone                        Availability Zone

                               Public subnet                            Public subnet
                                                                                              VPC Peering
                                                     Application Load
                                  NAT Gateway           Balancer           NAT Gateway

                                Private subnet                          Private subnet
                                                                                                            VPC


                               Private subnet                           Private subnet


                                     P                                      S
                                                                                              VPC Peering
```


## Region B
*(Slide 369)*

```text
                                                              VPC
                                                                                              Transit Gateway



                                                                                                                          Transit Gateway




                    Region A




                                                                                                      TGW Peering
                      VPC                                                                                           VPC


                                 Availability Zone                        Availability Zone

                               Public subnet                            Public subnet


                                                     Application Load
                                  NAT Gateway                              NAT Gateway
                                                        Balancer

                                Private subnet                          Private subnet
                                                                                                                    VPC
                                                                                              Transit Gateway

                               Private subnet                           Private subnet


                                     P                                      S
```


## Transit Gateway
*(Slide 370)*

- Allows customers to interconnect thousands of VPCs and on-premises networks.                             Region
- It’s a regional router
- Hub and spoke architecture where we can DC connect                                                   VPC
  - VPCs
  - VPN connection
  - Direct Connect Gateway                   Transit Gateway VPN or Direct
  - A Connect SD-WAN/third-party network                         Connect appliance
  - Another Transit Gateway (TGW peering)
- Ideal for centralized traffic inspection and Hybrid network setup etc. VPC


## Transit Gateway – Advanced features
*(Slide 371)*

- Supports IP Multi-cast
- AZ Affinity - tries to keep the traffic in the same        Region
AZ where the traffic originated. Use Appliance mode to influence the AZ selection. DC
- Supports sharing Transit Gateway across AWS               VPC accounts using Resource Access Manager (RAM)
- Architectures:                                        Transit Gateway VPN or Direct
  - Centralized VPC endpoint                                             Connect
  - Centralized Egress VPC
  - Centralized Traffic Inspection using Gateway Load Balancer
  - Centralized Traffic Inspection using AWS             VPC Network Firewall


## IP Multicast
*(Slide 372)*

- Multicast is a communication protocol used for delivering a single stream of data to multiple receiving computers simultaneously.
- Single/multiple sources and destinations
- Destination is a multicast group address:
  - Class D - 224.0. 0.0 to 239.255. 255.255
- Connectionless UDP based transport
- One way communication
- Examples: Sending email to the email-list, Conference call / Group chat, OTT platforms / TV Media, Stock exchange transaction updates https://en.wikipedia.org/wiki/Multicast
- Enable Transit Gateway for Multicast services while creating the transit gateway


## Transit Gateway – AZ Affinity
*(Slide 373)*

```text
                     VPC
                                      1
                           A
                                                                                     Source and destination in same AZ
      AZ1
                                          TGW ENI
                                      8
                                                         2
                                                                                          VPC
                                                                                                   5
                                                    7                       6                          4
      AZ2                                 TGW ENI                                3                                           AZ1
              10.1.0.0/16 (VPC 1)                                                       TGW ENI        5         Appliance
                                                                                 6                 4
                    VPC                             7                        3
                                  8                                                                                          AZ2
                           B                                                            TGW ENI                 Appliance
      AZ1                                           2
                                          TGW ENI
                                      1
                                                                                                     192.168.0.0/16
                                                                                                  (Shared Service VPC)
      AZ2                                 TGW ENI
                                                        Transit Gateway attempts to keep the traffic in the in the originating
              10.2.0.0/16 (VPC 2)                       Availability Zone until it reaches its destination
```


## Transit Gateway – AZ Affinity
*(Slide 374)*

```text
                     VPC
                                    1
                               A
                                                                    Source and destination in different AZ
      AZ1
                                          TGW ENI

                                   8                        2
                                                                           VPC
                                                                                         4
      AZ2                                 TGW ENI               3                                              AZ1
                                                    7
              10.1.0.0/16 (VPC 1)                                       TGW ENI             5      Appliance
                                                                6
                                                                       6                5
                    VPC                             7
                                                                3
                                                                                         4                     AZ2
                                                                        TGW ENI                   Appliance
      AZ1
                                         TGW ENI
                               8                        2
                                                                                       192.168.0.0/16
                                    1                                               (Shared Service VPC)
                           B
      AZ2                                 TGW ENI

              10.2.0.0/16 (VPC 2)                                                This causes Asymmetric Routing
```


## Transit Gateway – AZ Affinity
*(Slide 375)*

```text
                     VPC
                                    1
                           A
      AZ1
                                          TGW ENI
                                    8                                                        Appliance Mode Enabled
                                                          2
                                                                                               VPC
                                                                                                         5
                                                    7                            6                            4
      AZ2                                 TGW ENI                                     3                                                 AZ1
              10.1.0.0/16 (VPC 1)                                                           TGW ENI           5          Appliance
                                                                                      6                   4
                    VPC                             7                             3
                                                                                                                                        AZ2
                                                                                             TGW ENI                     Appliance
      AZ1
                                         TGW ENI
                               8                          2
                                     1                                                                     192.168.0.0/16
                                                                                                        (Shared Service VPC)
                           B
      AZ2                                 TGW ENI
                                                        When appliance mode is enabled, a transit gateway selects a single network interface in the
              10.2.0.0/16 (VPC 2)                       appliance VPC, using a flow hash algorithm, to send traffic to for the life of the flow.
```


## Transit Gateway – Route tables
*(Slide 376)*


## Flat network
*(Slide 377)*

```text
                                                           10.1.0.0/16                     10.2.0.0/16                      10.3.0.0/16               10.4.0.0/16



                                                Route           Destination     Route           Destination          Route        Destination      Route         Destination
                                              10.1.0.0/16     local           10.2.0.0/16     local               10.2.0.0/16     local          10.2.0.0/16     local
                    Static route
                       entry                  10.0.0.0/8      tgw-xxxxxxxx    10.0.0.0/8      tgw-xxxxxxxx        10.0.0.0/8      tgw-xxxxxxxx   10.0.0.0/8      tgw-xxxxxxxx


                                                                                                    Full Connectivity



                                                                                            Route            Destination        Prop.
                              AWS Transit             Default Route                   10.1.0.0/16       vpc-att1-xxxxxxxx       Yes
                               Gateway                                                10.2.0.0/16       vpc-att2-xxxxxxxx       Yes                Propagated
                                                          Table                                                                                    route entry
      Routing                                                                         10.3.0.0/16       vpc-att3-xxxxxxxx       Yes
      Domain                                                                          10.4.0.0/16       vpc-att4-xxxxxxxx       Yes
```


## Segmented Network
*(Slide 378)*

```text
                                                           10.1.0.0/16                     10.2.0.0/16                                  10.3.0.0/16                          10.4.0.0/16



                                              Route          Destination         Route         Destination                            Route           Destination                 Route       Destination
                                          10.1.0.0/16        local           10.2.0.0/16       local                              10.3.0.0/16         local               10.4.0.0/16         local
                                          192.168.0.0/16     tgw-xxxxxxxx    192.168.0.0/16    tgw-xxxxxxxx                       192.168.0.0/16      tgw-xxxxxxxx        192.168.0.0/16      tgw-xxxxxxxx


                                                                                              No East-West Connectivity




                                                                                                              Full Connectivity
                                                                                           Route             Destination                        Prop.
                                                        Routing domain for
                                                                                    192.168.0.0/1      vpn-att-xxxxxxxx                         Yes
                                                        VPCs                        6
                         AWS Transit                                                                                                            Route            Destination          Prop.
                                                        Routing domain for
                          Gateway                       VPN                                                                                10.1.0.0/16        vpc-att1-xxxxxxxx       Yes
                                                                                                                                           10.2.0.0/16        vpc-att2-xxxxxxxx       Yes
                                                                                                                                           10.3.0.0/16        vpc-att3-xxxxxxxx       Yes
                                                                                                                                           10.4.0.0/16        vpc-att4-xxxxxxxx       Yes
                                                                                      192.168.0.0/16
                                                                                                                                   VPN
```


## VPC Peering
*(Slide 379)*

*vs*

*VPC endpoint (PrivateLink)*


## Accessing Application privately across VPCs
*(Slide 380)*

```text
                                              Consumer VPC    Service Provider VPC




                                                              Load Balancer
                                                     Client
```


## Using VPC Peering
*(Slide 381)*

- VPC peering enables full layer 3 connectivity (and not application-level connectivity)
- Two-way communication
- Can not have VPC peering between VPCs having overlapping CIDRs
10.10.0.0/16                                            10.10.0.0/16 Consumer VPC                                     Service Provider VPC
Layer 3 connectivity
VPC Peering Load Balancer Client


## Using VPC Peering
*(Slide 382)*

- There is a limit on number of VPC peering connections (as of now 125 connections)
Service Provider VPC
VPC A
VPC Peering VPC B                         VPC Peering Load Balancer
VPC Peering VPC C


## Using VPC endpoint (PrivateLink)
*(Slide 383)*

- VPC endpoint + AWS PrivateLink = VPC Interface endpoint + VPC endpoint service
- Allows connecting to only Application Services privately
- No problem if Service provide and consume VPCs have overlapping CIDRs
VPC Endpoint Service 10.10.0.0/16                                          10.10.0.0/16 Consumer VPC                                 Service Provider VPC
Interface      AWS        Network Load Client Endpoint    PrivateLink     Balancer


## Using VPC endpoint (PrivateLink)
*(Slide 384)*

- Can connect Clients across thousands of consumer VPCs
- One-way communication – From client to the VPC endpoint service
One-way Service Provider VPC
Network Load AWS PrivateLink Balancer


## VPC Peering vs VPC endpoint (PrivateLink)
*(Slide 385)*

- VPC peering is useful when there are many resources that should communicate between peered VPCs
- VPC endpoint (AWS PrivateLink) should be used when you want to allow access to only single application hosted in your VPC to the clients in other VPCs (without peering the VPCs)
- When there is overlapping CIDRs, VPC peering connection cannot be created. However, AWS PrivateLink does support overlapping CIDR.
- We can create a maximum of 125 peering connections. There is no limit on AWS PrivateLink connections.
- VPC peering enables bidirectional traffic origin. AWS PrivateLink allows only consumer to originate the traffic.


## VPC private connectivity options
*(Slide 386)*

- VPC Peering connection
- AWS Transit Gateway
- AWS Site-2-Site VPN
- AWS Client VPN                 Hybrid Networking
- AWS Direct Connect


## Site-to-Site VPN
*(Slide 387)*

```text
                     Region
                                                                Internet Gateway

                       VPC                                                                   Internet


                                Availability Zone                        Availability Zone

                              Public subnet                            Public subnet
                                                                                                        Corporate Data Center
                                                                                                        (on-premises)
                                                    Application Load
                                NAT Gateway            Balancer           NAT Gateway

                               Private subnet                          Private subnet




                              Private subnet                           Private subnet


                                    P                                      S

                                                                                                             192.168.0.0/16
```


## Site-to-Site VPN
*(Slide 388)*

```text
                     Region
                                                                Internet Gateway

                       VPC                                                                                             Internet


                                Availability Zone                        Availability Zone

                              Public subnet                            Public subnet
                                                                                                                                                   Corporate Data Center
                                                                                                                                                   (on-premises)
                                                    Application Load
                                NAT Gateway            Balancer           NAT Gateway

                               Private subnet                          Private subnet




                              Private subnet                           Private subnet                          Site-to-Site IPSec VPN

                                    P                                      S
                                                                                             Virtual Private                            Customer
                                                                                                Gateway                                 Gateway         192.168.0.0/16
```


## https://youtu.be/e-kTDqcuVFQ
*(Slide 389)*


## AWS Site-to-Site VPN
*(Slide 390)*

- A Managed IPSec VPN connection between on-premises router and AWS VPC
- Customer Gateway (CGW) on Customer side and Virtual Private Gateway (VGW) on AWS side
- Traffic flows over the internet but encrypted at Layer 3
- 2 VPN Tunnels for High Availability.
- Single connection provides bandwidth of ~1.25 Gbps
- Supports Static Routing and Dynamic Routing (BGP)                                        Corporate Data Center (on-premises)
Region
VPC
ASN 65000                  ASN 65001 Subnet
VGW          Site-to-Site VPN           CGW 10.0.0.0/16 192.168.0.0/16


## https://youtu.be/LP_n396Rhy4
*(Slide 391)*


## VPN Architectures
*(Slide 392)*


## Primary and Failover VPN connections
*(Slide 393)*

```text
                  Region
                                                                          Private Data Center
                          VPC
                                    Availability Zone
                                                         Public IPs     Customer
                           Subnet                                       Gateway
                                                                      Public IP


                                                        VGW
                                    Availability Zone
                           Subnet
                                                                      Public IP
                                                         Public IPs     Customer
                                                                        Gateway
```


## VPN CloudHub – Routing between multiple customer sites
*(Slide 394)*

- Using VPN Gateway in detached mode                                                  Data Center CGW ASN: 65000   (Branch A)
- Each customer gateway must have unique BGP ASN with dynamic routing
- Sites must not have overlapping IP ranges
- Connect upto 10 Customer Gateways                                                   Data center (Branch B)
- Can serve as failover connection between on-premises locations CGW ASN: 65001 VPN gateway Data center (Branch C)
CGW ASN: 65002


## VPC private connectivity options
*(Slide 395)*

- VPC Peering connection
- AWS Transit Gateway
- AWS Site-2-Site VPN
- AWS Client VPN
- AWS Direct Connect


## AWS Client VPN
*(Slide 396)*

Client VPN
- Connect from your workstation privately to VPC using OpenVPN
- Encrypted traffic goes over the internet
- Access VPC resources over the Private IPs as if you are part of the same network Region Internet Gateway
VPC
Availability Zone                        Availability Zone
Public subnet                            Public subnet
Internet Application Load NAT Gateway            Balancer           NAT Gateway
Private subnet                          Private subnet
Client VPN endpoint Private subnet                           Private subnet
P                                      S


## Site-to-Site VPN
*(Slide 397)*

Region Internet Gateway                               •    Latency VPC                                                                                     •    Jitter Internet
- Inconsistent Availability Zone                        Availability Zone
Public subnet                            Public subnet                                                                   Corporate Data Center (on-premises)
Application Load NAT Gateway            Balancer           NAT Gateway
Private subnet                          Private subnet
Private subnet                           Private subnet                              Site-to-Site IPSec VPN
P                                      S Virtual Private                                Customer Gateway                                     Gateway         192.168.0.0/16


## Direct Connect
*(Slide 398)*

```text
             Region
                                                        Internet Gateway

               VPC


                        Availability Zone                        Availability Zone

                      Public subnet                            Public subnet
                                                                                                      Corporate Data Center
                                                                                                      (on-premises)
                                            Application Load
                        NAT Gateway            Balancer           NAT Gateway

                       Private subnet                          Private subnet


                                                                                     Direct Connect
                      Private subnet                           Private subnet


                            P                                      S

                                                                                     Direct Connect        192.168.0.0/16
                                                                                        Location
                                                                                        (in cities)
```


## Direct Connect (DX)
*(Slide 399)*

internet
AWS backbone                     Physical Connection network Direct Connect                          On-premises Data AWS Region Location                                  Center
- Provides dedicated and consistent network
- Provides network bandwidth from 50 Mbps up to 100 Gbps over a single connection
- Low data transfer cost
- May take up to 1-3 months to establish end-to-end connectivity.


## AWS Direct Connect (DX)
*(Slide 400)*

- AWS Direct Connect provides private, dedicated network connectivity between on-premises and AWS, bypassing the public internet
- Offers bandwidth options: 1 Gbps, 10 Gbps, 100 Gbps on dedicated connection, and 50 Mbps to multi-Gbps on hosted connection
- Fixed port-hour charges based on the port bandwidth
- Helps reduce data transfer costs, especially outbound traffic.
- Use Private VIF to access a VPC, Public VIF to privately access AWS public services like S3 or DynamoDB and Transit VIF to connect to Transit Gateway (via Direct Connect Gateway)
- Use cases: Large data migrations, hybrid workloads, latency-sensitive applications


## Direct Connect – Connection Types
*(Slide 401)*

- Dedicated Connections: 1Gbps, 10 Gbps and 100 Gbps capacity
  - Physical ethernet port dedicated to a customer
  - Can be either setup by your Network Provider or AWS Direct Connect Partner
- Hosted Connections:
  - 50, 100, 200, 300, 400, 500 Mbps and 1 Gbps, 2 Gbps, 5 Gbps, 10 Gbps
  - Connection requests are made via AWS Direct Connect Partners
  - AWS uses traffic policing on hosted connections – excess traffic is dropped


## AWS Direct Connect - Virtual Interfaces (VIFs)
*(Slide 402)*

```text
                   S3         SQS
                                                            Direct Connect Location

                                                                             Colocation
                                                                                          Corporate data
                                                                                          center


                                              Private VIF
                                                            AWS Direct       Customer
                                                             Connect          Router
                  VPC                                         Router




                          Transit Gateway
```


## Direct Connect Gateway
*(Slide 403)*

- Global network device – Accessible in all regions
- Direct Connect integrates via a private VIF or a transit VIF


## DX Gateway with Private VIF
*(Slide 404)*

```text
                                        Account #1    Direct              Direct Connect Location
           Region (Mumbai)                           Connect
                                                     Gateway


                        VPC A                 VGW                                                              Corporate data
                                                               Private                                         center
                                                                 VIF
           Region (N. Virginia)        Account #2
                                                                                      Customer      Customer
                                                                         AWS Direct
                                                                                       Router        Router
                                                                          Connect
                                                                           Router
                        VPC B                 VGW



                       VPC VC                 VGW
```


## DX Gateway with Transit VIF
*(Slide 405)*

```text
           Region


                 AWS VPC 1                                                                             Corporate data
                                                                                                       center

                                              Transit Gateway


                                                                            Transit
                                                                             VIF
          Region
                    AWS VPC 2                                   Direct Connect        AWS Direct
                                                                   Gateway             Connect
                                                                                        Router

                    AWS VPC 3
                                              Transit Gateway


                                                                             Direct Connect Location
```


## AWS Direct Connect – Important to know
*(Slide 406)*

- AWS Direct Connect provides private, dedicated network connectivity between on-premises and AWS.
- Uses BGP for Dynamic routing
- Offers high bandwidth options: 1 Gbps, 10 Gbps, 100 Gbps on dedicated ports, and hosted DX from 50 Mbps to multi-Gbps.
- Traffic is not encrypted by default; for encryption use VPN over DX or MACsec (where supported).
- It takes few weeks to month to establish Direct Connect connectivity (hence need advance planning)
- Use Private VIF to access a VPC, Public VIF to privately access AWS public services like S3 or DynamoDB and Transit VIF to connect to Transit Gateway (via DX Gateway)
- Use DX Gateway to connect a single DX to multiple VPCs across multiple Regions.
- Provides reduce data transfer costs (e.g. $0.02/GB as compared to internet charge of $0.09/GB)
- Architecture: For high availability, deploy multiple DX connections in different DX locations;
- Architecture: You can use DX+VPN for resilient connectivity where VPN acts as a failover network.


## VPC Traffic Monitoring
*(Slide 407)*

*VPC Flow logs and Traffic mirroring*


## VPC Flow Logs
*(Slide 408)*

- Capture information about IP traffic going in/out of your elastic network interfaces (ENI):
  - Elastic Network Interface Flow Logs
  - Subnet Flow Logs
  - VPC Flow Logs
- Helps to monitor & troubleshoot connectivity issues
  - Troubleshooting connectivity issues (e.g. “Why can't my EC2 talk to RDS?").
  - Identifying blocked traffic by Security Groups or NACLs
  - Detecting anomalous or suspicious traffic (e.g. unusually high egress).
  - Validating firewall rules and network configurations across subnets/VPCs.
  - Cost analysis by checking high bandwidth usage.
- Also captures network information from AWS managed interfaces: ELB, RDS, ElastiCache, Redshift, Amazon WorkSpaces etc.
- There is no impact to network performance for enabling VPC flow logs.
- Flow logs data can be sent to S3 / CloudWatch Logs / Kinesis Data Firehose for storage and analysis


## VPC Flow logs – default format
*(Slide 409)*

```text
                                                                                                                packets   bytes   start   end     action
                                                                                                                                                log-status
               version         account-id     interface-id   srcaddr   dstaddr   srcport   dstport   protocol
```


## Publishing VPC flow logs
*(Slide 410)*

```text
                 VPC

                         Private subnet
                                                                                CloudWatch
                                              Flow logs
                                                               CloudWatch       Logs Insights


                       ENI       ENI Flow logs


                                                  Flow logs
                          Private subnet                        Amazon S3      Amazon Athena



                                   ENI
                                                                                                                 +
                                                                                 S3     Redshift   OpenSearch
                                                              Amazon Kinesis
                                                               Data Firehose
```


## VPC Traffic Mirroring
*(Slide 411)*

- Copies network traffic from an elastic network interface of Amazon EC2 instances
- Use cases:
  - Advanced troubleshooting that requires full payload visibility.
  - Deep packet inspection (DPI) for security appliances.
  - Routing mirrored packets to third-party security tools (e.g. Palo Alto, Check Point).
- How to set up Traffic Mirroring (via AWS VPC console)
  - Create the Mirror Target -> Define the Traffic Filter -> Create Mirror Session
- Mirror source is ENI and target could be another ENI or Network Load Balancer (UDP - 4789)
- Filter the traffic to be mirrored by protocol, source/dest port, CIDR
- Source and destination can be in the same VPC or across VPCs which are connected via VPC peering or Transit gateway
- Source and destination VPCs can be in different AWS accounts


## VPC Traffic Mirroring – ENI as Target
*(Slide 412)*

```text
                             VPC



                                   Subnet A                                            Subnet B




                                                    Traffic Mirroring                               Traffic
                                   Traffic Source                                                 Destination Traffic Analyzer
                                                                                                    (ENI)




                                                                        10.10.0.0/16
```


## VPC Traffic Mirroring – NLB as Target
*(Slide 413)*

```text
                             VPC                                             VPC                         Subnet B


                                  Subnet A

                                                                          UDP Port 4789




                                                      Traffic Mirroring     Traffic
                                     Traffic Source                       Destination
                                                                            (NLB)
                                       10.10.0.0/24
                                                                                                          Traffic Analyzer


                                              10.10.0.0/16                                10.20.0.0/16
```


## VPC Flow logs vs Traffic mirroring
*(Slide 414)*

VPC Flow Logs                                VPC Traffic mirroring
- Captures only traffic metadata (source/destination,
- Captures actual packet payloads ports, allow/deny)
- Used for deep packet inspection, intrusion detection
- Used for troubleshooting network connectivity issues       and threat investigations
- Logs can be stored in Amazon CloudWatch, Amazon S3
- Sends mirrored traffic to 3rd party Network security or can be streamed using Kinesis Data Firehose for         appliances for real-time analysis further analysis.
- Heavy payload
- Lightweight
- High cost
- Low cost


## IPv6 in VPC
*(Slide 415)*


## IPv6 in VPC
*(Slide 416)*

- VPC supports dual-stack mode
- Resources inside VPC can communicate over IPv4 or IPv6 or both
- IPv4 support can not be disabled for the VPC
- VPC supports IPv6-only subnets
- IPv6 addresses are Public by default


## Outbound Internet access for instance in a Private subnet (IPv6)
*(Slide 417)*

```text
                                                VPC                                                 10.10.0.0/16
                                                                                         2001:db8:1234:1a00::/56

                                                      Public subnet                      Private subnet
                                                                 10.10.0.0/24                      10.10.1.0/24
                                                      2001:db8:1234:1a00::/64           2001:db8:1234:1a01::/64

     252.1.2.3                                                                IPv4 to IPv4
                                          IGW
                                                                                 traffic
                                                                                                                        Destination          Target
                                                                          X   IPv6 to IPv6
        User                                                                                                             10.0.0.0/16          Local
                                                            NAT Gateway          traffic
  2001:1:2:3:4:5:6:7                                        11.22.33.44                       10.10.0.15           2001:db8:1234:1a00::/56    Local
                                                                                      2001:db8:1234:1a01::a               0.0.0.0/0          NAT-GW
```


## Egress-only internet gateway
*(Slide 418)*

```text
                                                   VPC                                                 10.10.0.0/16
                                                                                            2001:db8:1234:1a00::/56

                                                         Public subnet                    Private subnet
                                                                    10.10.0.0/24                    10.10.1.0/24
                                                         2001:db8:1234:1a00::/64         2001:db8:1234:1a01::/64

     252.1.2.3                            IGW                                IPv4 to IPv4
                                                                                traffic                                    Destination            Target

        User                                                               X IPv6 to IPv6                                   10.0.0.0/16           Local
                                                               NAT Gateway      traffic
  2001:1:2:3:4:5:6:7                                                                       10.10.0.15                 2001:db8:1234:1a00::/56     Local
                                                               11.22.33.44
                                     Egress-only                                        2001:db8:1234:1a01::a                0.0.0.0/0           NAT-GW
                                        IGW
                                                                          IPv6 to IPv6 traffic                                 ::/0             Egress-IGW
```


## IPv6 in VPC – Important to know
*(Slide 419)*

- IPv6 provides globally unique, publicly routable 128-bit addresses
- IPv6 addressing in VPC uses /56 at VPC level and /64 per subnet, which is the minimum required size for IPv6 routing.
- NAT Gateway does not work with IPv6; instead, IPv6 uses an Egress-Only Internet Gateway, which allows outbound-only IPv6 traffic from instances while blocking incoming IPv6 connections from the internet.
- Security Groups and NACLs support separate IPv4 and IPv6 rules (::/0), so you must explicitly Allow or Deny IPv6 traffic.
- Dual-stack mode lets you run IPv4 and IPv6 together in the same VPC and subnets, improving compatibility during migration.
- VPC Endpoints (Interface/Gateway) support IPv6 in many regions, allowing private IPv6 access to AWS services.
- Route tables include separate IPv6 routes, commonly pointing to Egress-Only IGW or Internet Gateway depending on inbound/outbound needs.
- Useful for IoT, global-scale workloads, and scenarios needing large address space or avoiding NAT bottlenecks.


## Let’s talk Architecture
*(Slide 420)*


## VPC architecture for hosting simple Web Application
*(Slide 421)*

```text
                                                                                                    Internet




                                              Region


                                                                                                    Internet Gateway
                                                                                                                           10.0.0.0/16
                                                       VPC
                                                                                                               2001:db8:1234:1a00::/56

                                                              Availability Zone                           Availability Zone

                                                             Public subnet                               Public subnet


                                                                                  Application Load                   NAT Gateway
                                                       NAT Gateway                   Balancer

                                                              Private subnet                              Private subnet




                                                             Private subnet                               Private subnet


                                                                  P                   replication               S
```


## VPC Gateway endpoint and Interface endpoints
*(Slide 422)*

```text
                                                                                                                   Internet




                                              Region


                                                                                                                   Internet Gateway
                                                                                                                                          10.0.0.0/16
                              Kinesis                                 VPC
                                                                                                                              2001:db8:1234:1a00::/56
                                                 PrivateLink
                                                                             Availability Zone                           Availability Zone
                                SQS
                                                                            Public subnet                               Public subnet
                                                       VPC Endpoint
                                SNS                     (interface)                              Application Load                   NAT Gateway
                                                                      NAT Gateway                   Balancer

                                                                             Private subnet                              Private subnet


                                   S3

                                                                            Private subnet                               Private subnet
                             DynamoDB
                                                       VPC Endpoint              P                   replication               S
                                                        (gateway)
```


## Connecting multiple branch offices over Site-to-Site VPN
*(Slide 423)*

```text
                                                                Corporate data center

                          VPC


                                                                Corporate data center


                          VPC


                                                                Corporate data center



                          VPC

                                                                Corporate data center




                                              VPN Connections
```


## Simplify Site-to-Site VPN network with Transit Gateway
*(Slide 424)*

```text
                                                                                                   Corporate data center

                          VPC
                                                                                  VPN Connection


                                               VPC attachment                                      Corporate data center

                                                                VPN attachments
                          VPC                                                     VPN Connection


                                                                                                   Corporate data center
                                              Transit Gateway
                                                                                  VPN Connection
                          VPC
                                               VPC attachment
                                                                                                   Corporate data center


                                                                                  VPN Connection
```


## Multiple VPN Connections over VGW – Limited bandwidth
*(Slide 425)*

Corporate data center
VPN Connection (Primary) VPC Internal routing and failover
VPN Connection Virtual Private Gateway         (failover) (VGW)
1.25 Gbps
- VPN connection throughput 1.25 Gbps
- Virtual Private Gateway does not support ECMP.


## Transit Gateway & VPN - Higher aggregate bandwidth
*(Slide 426)*

ECMP = Equal Cost Multi-path Corporate data center
VPN Connection BGP + ECMP            (active) VPC Dynamic routing, asymmetric routing, ECMP Transit         VPN Connection Gateway             (active)
- TGW – Enable BGP and ECMP for VPN connections.
- 2 VPN connection x 2 Tunnels per connection x 1.25 Gbps/tunnel = ~5 Gbps


## Single DX Connection: VPN as a backup
*(Slide 427)*

```text
                                                                 Internet


                                                    Direct Connect Location

                                              VPN                   Colocation
                                                                                 Corporate
             VPC                                                                 data center

                                                    AWS Direct       Customer
                                                     Connect          Router
                                                      Router
```


## Direct Connect maximum resiliency
*(Slide 428)*

```text
                                              Direct Connect Location 1   Maximum
                                                                          Resiliency   Corporate
                                                                                       data center

                                                          Customer
                                              Device #1   Router #1




                                                           Customer
                                              Device #2    Router #2



                                              Direct Connect Location 2
                                                                                       Corporate
                                                                                       data center
                                                          Customer
                                              Device #1   Router #1




                                                          Customer
                                              Device #2   Router #2
```


# Amazon S3

*(Source: Slide 429)*


## AWS Storage services
*(Slide 430)*

```text
                 Block                                                 File                                 Object




                           EC2                Amazon    Amazon Amazon Amazon Amazon Amazon                  Amazon
         Amazon                                         FSx for FSx for   FSx       FSx
                         instance              EFS                                        File Cache          S3
          EBS                                           NetApp Windows for Lustre   for
                          Storage
                                                        ONTAP File Server         OpenZFS



                                              Hybrid Storage and Data Transfer                                  Backup

                Hybrid Storage                 Online
                                                                         Data Transfer
                                                                                          Offline

                        AWS Storage
                                                                                                                  AWS
                         Gateway
                                                AWS DataSync     AWS Transfer Family         AWS Snowball        Backup
```


## Web
*(Slide 431)*

```text
                                                        Users                                 Browser
         myapp.com on AWS                                                                                                     CloudFront

                                                         Route53            myapp.com                                                            Edge
                                                                                                                                               Locations



                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                     Lambda

                                                Web                                                                  Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                            Storage Convert Storage
                                                                    S           S                Rekognition   S3             S3
                            SNS
                                                                                                           AI models                                   QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker


                            SQS
                                              ElastiCache
                                                                                                                      Storage
                                                                               Neptune        Kinesis                    S3              EMR
                                                                                                                                                           Redshift
                       CloudWatch                  Multi-AZ




                                                              RDS       DynamoDB              Glue
```


## Amazon S3 features
*(Slide 432)*

Logging, Storage             Event based            Monitoring and         Performance Storage Classes                          Security        Management            automation               reporting            Optimization
Different storage                   Auditing and          To manage costs,     To transform data     To help gain visibility To improve the types based on                      managing access       reduce latency and   and trigger           into storage usage to performance of S3 durability, availability,           to your buckets       meet regulatory      workflows to          better understand,      storage, access, access pattern,                     and objects           requirements like    automate a variety    analyze, and optimize latency and more latency and cost                                          data residency,      of data processing    storage at scale requirements                                              retention etc.       activities at scale
- S3 Standard
- S3 Block Public
- S3 Lifecycle
- S3 Event
- Server Access
- Prefix partition
- S3 Standard-IA                              Access
- S3 Replication      Notification          Logging
- Multi-part uploads
- S3 One Zone-IA
- S3 Bucket Policy
- S3 Batch
- Amazon
- S3 Inventory
- S3 Byte range
- S3 Intelligent Tiering
- S3 Encryption      Operations          EventBridge
- IAM Access
- S3 Transfer
- Glacier Instant Retrieval
- S3 Versioning
- S3 Object             Analyzer             Acceleration
- Glacier Flexible
- Object Lock                            Lambda
- S3 Storage Lens
- Caching with Retrieval                •                  MFA Delete                                                                        CloudFront
- Glacier Deep Archive
- S3 Express One Zone


## Amazon S3 – Additional features
*(Slide 433)*

- Static Website Hosting
- Requester Pays
- S3 Pre-signed URL
- Cross Origin Resource Sharing (CORS)
- S3 Access Points


## Block storage / File storage / Object Storage
*(Slide 434)*


## Quick look at storage performance parameters
*(Slide 435)*

Storage Performance parameters – IOPS, Throughput, Latency
- What is IOPS? – Input Output operations per second e.g. 3000 IOPS
- What is Throughput? – How fast storage can read/write data e.g. 10 MB/s
- What is Latency? – Time delay between request of data and response of data
~250 Mbps Aggregate throughput ~10 Gbps


## AWS Storage services
*(Slide 436)*

EBS                           EFS/FSx                           S3
- Block Storage – Data is
- Shared file system –
- Object storage – Flat structure stored into unique blocks          Hierarchical structure      •   API access to data (HTTPS)
- Host File System places data
- There is a serving File
- Metadata driven (Attributes, on Disk using protocols like       system                          Policy)
- Should be mounted on EC2
- High Throughput, unlimited
- Must be attached to EC2            or on-premises servers          storage, moderate performance
- High performance, high
- High throughput, moderate IOPS, low latency                  performance


## How to access?                                                          Web
*(Slide 437)*

```text
                                                                                  Browser




                                                                      Availability Zone 1      Availability Zone 2     Availability Zone 3




                                                                      EC2        Containers   EC2        Containers   EC2
  Block Storage




                             Elastic Block Store                     Volume          Volume   Volume       Volume     Volume
                                    (EBS)
                                                             https
  File Storage




                   Elastic File    FSx                                      mount                   mount                   mount
                                                   Filesystem
                    System          for
                     (EFS)        Windows                              https
  Object Storage




                         Simple Storage Service     Bucket
                                  (S3)
```


## Amazon S3
*(Slide 438)*

- One of the early and most powerful service launched by AWS (in 2006)
- Amazon S3 is a web-based object storage service which provides virtually unlimited space
- Designed for 99.999999999% durability.
- Customers of all sizes and industries use S3 for range of use cases.
- Ideal for wide variety of use cases including:
  - Enterprise Applications
  - Data Lake
  - Media store
  - Backup and Archive
  - Big data Analytics
  - Static websites


## S3 Bucket and Objects
*(Slide 439)*


## Bucket
*(Slide 440)*

- For storing data in Amazon S3, we need to create S3 Bucket.
- Buckets are regional - all data is stored in the AWS Region where the bucket is created.
- Bucket names must be globally unique across all AWS customers.
- By default, all buckets and objects are private - no public access unless explicitly allowed.
- Amazon S3 supports creating different types of the buckets:
  - General Purpose bucket - Most common and widely used
  - Directory Bucket - Designed for high performance workloads
  - Table Bucket - For tabular datasets, Optimized for Apache Iceberg table format Bucket Name: awswithchetan
  - Vector Bucket - For storing vector embeddings for ML and LLM models


## Object
*(Slide 441)*

- Key – Uniquely identifies the object in the bucket
- Metadata - Set of name-value pairs that is stored along with the object.
  - Amazon S3 also assigns system-metadata to these objects, which it uses for managing objects such as creation time, size, storage class etc.
  - You can assign user-defined metadata to your objects e.g. format=json
- Version ID – If versioning is enabled for the bucket, Version ID uniquely identifies the specific version of an object.
- Access control information - Object ACL for granular access
- Tags - User defined key-value pairs that can be used for access control in IAM policies, cost allocation etc.
- ETAG – System generated hash (mostly MD5) but not always
Bucket: awswithchetan Object Key: section5/s3-overview.pdf Object URI: s3://awswithchetan/section5/s3-overview.pdf Object URL: https://awswithchetan.s3.ap-south-1.amazonaws.com/section5/section5/s3-overview.pdf


## Object User-defined Metadata vs Tags
*(Slide 442)*

User-defined Metadata                                                  Tags
- Key-value pairs stored inside the object’s metadata.
- Key-value pairs stored separately from object metadata.
- Added only at the time of upload (PUT or POST).
- Can be added, updated, or removed anytime
- Cannot be modified later without re-uploading the object.
- Not returned with the object payload (separate API)
- Keys are stored as HTTP headers and sent back on every
- Used by AWS services for automation and governance: GET request.                                                       •   Lifecycle transitions
- Used mostly by applications to store custom information.
- Expiration
  - Replication
- Examples:
  - Object Lock retention rules
  - x-amz-meta-source: camera-1
  - Access controls (IAM conditions)
  - x-amz-meta-resolution: 1080p
  - S3 Inventory filtering
- Examples:
  - env=development
  - tier=premimum


## How to use S3?
*(Slide 443)*

upload                                        access
- Object key
Users admin                              create bucket
  - Bucket name
  - AWS Region
  - More..
- Admin must have permissions to create bucket and upload object
- Users must have permissions to read objects from this bucket


## Exercise - Create bucket and upload file
*(Slide 444)*

```text
                                              1   Create a S3 bucket in region of your choice

                                              2   Upload an object (image/video/pdf anything)

                                upload
```


## Amazon S3 Features
*(Slide 445)*


## Amazon S3 features
*(Slide 446)*

Logging, Storage             Event based            Monitoring and         Performance Storage Classes                          Security        Management            automation               reporting            Optimization
Different storage                   Auditing and          To manage costs,     To transform data     To help gain visibility To improve the types based on                      managing access       reduce latency and   and trigger           into storage usage to performance of S3 durability, availability,           to your buckets       meet regulatory      workflows to          better understand,      storage, access, access pattern,                     and objects           requirements like    automate a variety    analyze, and optimize latency and more latency and cost                                          data residency,      of data processing    storage at scale requirements                                              retention etc.       activities at scale
- S3 Standard
- S3 Block Public
- S3 Lifecycle
- S3 Event
- Server Access
- Prefix partition
- S3 Standard-IA                              Access
- S3 Replication      Notification          Logging
- Multi-part uploads
- S3 One Zone-IA
- S3 Bucket Policy
- S3 Batch
- Amazon
- S3 Inventory
- S3 Byte range
- S3 Intelligent Tiering
- S3 Versioning      Operations          EventBridge
- IAM Access
- S3 Transfer
- Glacier Instant Retrieval
- S3 Encryption
- S3 Object             Analyzer             Acceleration
- Glacier Flexible
- Object Lock                            Lambda
- S3 Storage Lens
- Caching with Retrieval                •                  MFA Delete                                                                        CloudFront
- Glacier Deep Archive
- S3 Express One Zone


## S3 Storage Classes
*(Slide 447)*


## S3 Storage Classes
*(Slide 448)*

Frequently Accessed Objects Automatically move Data across storage classes
- S3 Standard (General Purpose)
- S3 Intelligent-Tiering
- 11 9’s (99.999999999%) of durability
Archiving Objects – S3 Glacier Infrequently Accessed Objects
- S3 Glacier Instant Retrieval
- Standard-IA
- S3 Glacier Flexible Retrieval
- S3 One Zone-IA
- S3 Glacier Deep Archive
S3 Standard             S3 Standard-IA   S3 One Zone-IA S3 Intelligent-      S3 Glacier          S3 Glacier         S3 Glacier Tiering        Instant Retrieval   Flexible Retrieval   Deep Archive


## S3 Storage Classes
*(Slide 449)*

```text
                                                                                                     S3 Glacier          S3 Glacier         S3 Glacier
                                S3 Standard   S3 Intelligent-   S3 Standard-IA   S3 One Zone-IA   Instant Retrieval   Flexible Retrieval   Deep Archive
                                                 Tiering

 Access Latency                 Low                     < single-digit millisecond                            > minutes to hours                  High

Access Frequency                High                                 once a quarter          once or twice in a year          < once a year       Low

Storage cost                    High                                                                                                               Low

Data Retrieval cost                                                Low                                                                             High

 Number of requests Low                                                                                                                            High
       cost
```


## Data storage cost
*(Slide 450)*

```text
         *N. Virginia region, initial tier



                                                                Cost per GB per month



                                                                   As per storage
                                                                       class
                                              30 days                    +                       90 days                    180 days
                                                                   monitoring cost
                                                                   of $2.5 per 1M
                                                                       objects                                              $0.00099
          $0.023                   $0.0125              $0.01                           $0.004             $0.0036




      S3 Standard            S3 Standard-IA       S3 One Zone-IA S3 Intelligent-    S3 Glacier            S3 Glacier         S3 Glacier
                                                                    Tiering      Instant Retrieval     Flexible Retrieval   Deep Archive
```


## Bulk
*(Slide 451)*

```text
         Data Retrieval time/cost
                                                                                                                                                                         In 48 hours
                                                                                                                                                 Bulk
                                                                                                                                              5-12 hours    Standard
                                                                                                                                                           In 12 hours
                                                                                                                                                                                $
                                                                                                                                                                   $
                                                                                                                                     Standard
                                                                                                                                     3-5 hours


                                                                                    Depends on underlying
                                                                                                                                              $
                                                                                                                              Expedited
                                                                                        storage class                          1-5 min
                                                      $                         $                                                   $
                                    In milliseconds
         In milliseconds




                                                              In milliseconds




                                                                                                            In milliseconds
                                                                                      In milliseconds




                                                                                                                                 minutes to




                                                                                                                                                             hours to
                                                                                                                                   hours




                                                                                                                                                              days
      S3 Standard            S3 Standard-IA               S3 One Zone-IA S3 Intelligent-    S3 Glacier                               S3 Glacier                S3 Glacier
                                                                            Tiering      Instant Retrieval                        Flexible Retrieval          Deep Archive
```


## S3 Storage Classes
*(Slide 452)*

```text
                                                                                                     S3 Glacier          S3 Glacier         S3 Glacier
                                S3 Standard   S3 Intelligent-   S3 Standard-IA   S3 One Zone-IA   Instant Retrieval   Flexible Retrieval   Deep Archive
                                                 Tiering

 Access Latency                 Low                     < single-digit millisecond                            > minutes to hours                  High

Access Frequency                High                                 once a quarter          once or twice in a year          < once a year       Low

Storage cost                    High                                                                                                               Low

Data Retrieval cost                                                Low                                                                             High

 Number of requests Low                                                                                                                            High
       cost
```


## S3 Storage Classes – Use cases
*(Slide 453)*

S3 Standard                        S3 Intelligent-          S3 Standard-IA           S3 One Zone-IA            S3 Glacier Tiering
- Common
- For unknown,
- Hot backups
- Storing secondary
- Backup and Archive
- Data for disaster       backup copies of on-
- Long term storage
- Mobile and Gaming                      unpredictable data        recovery                premise data              for regulatory and
- Older media
- Storing data that you     compliance
- Media Content &
- Data lakes, data          contents, logs          can recreate distribution                           analytics


## S3 Storage classes - Summary
*(Slide 454)*

*https://aws.amazon.com/s3/storage-classes/*


## S3 Intelligent Tiering
*(Slide 455)*


## Amazon S3 Intelligent-Tiering
*(Slide 456)*

```text
       For data with unknown or changing access patterns



                                                                                 Archive                 Archive                  Deep
              Frequent                            Infrequent
                                                                                  Instant                Access                  Archive
             Access tier                          Access tier
                                                                                Access tier                tier                 Access tier
                                     +30 days                        +60 days                 +90 days              +180 days




                                              Automatic Transition                                  Optional Transition
                                                                                                      (configurable)



                                 Moves objects between three access tiers for a small monthly monitoring
                                                          and automation fee
```


## S3 Express One Zone
*(Slide 457)*

- Provides High-performance with single-digit millisecond data access at scale Region
- Single-Availability Zone storage class where you can select the specific AZ in which bucket should be created.                                      Availability Zone   Availability Zone
- Designed specifically to work with S3 Directory Buckets (a new bucket type optimized for low-latency, high-request-rate operations).
- Delivers consistent data access with over 2 million requests per second
- Data access speed up to 10x faster and request costs up to 80% lower                           S3 Express One than S3 Standard                                                                                    Zone
- You can choose to co-locate your storage and compute resources (EC2, EKS etc.) in the same Availability Zone
- Ideal for low latency storage access requirement applications like ML and AI model training                                                                              EC2


## Amazon S3 Security
*(Slide 458)*


## S3 Security
*(Slide 459)*

Access or Permission layer Security
  - Block Public Access
  - Access Control List (ACL)                                  Cross-Account
  - Bucket Policy
    - Restrict access to IAM user/role                                                   VPC
    - Cross-account access
    - Restrict access from VPC or IP address     AWS IAM User
Data layer Security                                   AWS IAM Role                                Client IP S3 Bucket
- Encryption
- Object Lock
- Data deletion protection (MFA) Internet
- S3 Versioning                                   Federated User Anonymous User


## S3 Block Public Access
*(Slide 460)*

- To prevent company data leaks
- Set at the account level or bucket level
- Best practice: Leave these ON unless bucket needs to be publicly accessible


## Access Control List (ACL)
*(Slide 461)*

- Each bucket and object has an ACL attached to it
- ACL controls which AWS accounts or groups are granted access and the type of access
- Bucket owner can enable/disable ACLs
- ACLs are disabled by default
Bucket Level ACL                 Object Level ACL
A majority of modern use cases in Amazon S3 no longer require the use of ACLs.


## Bucket Policy
*(Slide 462)*

```text
                           IAM User or Role policy                     S3 Bucket Policy




                                                               Bucket policy
                                      IAM policy




                                                                                          Anonymous
                                              Does user have                              user on the
                                               permission?              Does bucket
                                                                                            internet
                                                                       allow users to
                                                                          access?
```


## Bucket Policy
*(Slide 463)*

It’s a JSON-based policy
- Resources: buckets and objects
- Effect: Allow / Deny
- Actions: Set of API to Allow or Deny
- Principal: The account or user to apply the policy to
- Condition: Apply policy when condition is true
Examples of bucket policy:
- Grant public read access to the bucket e.g. website
- Objects must be encrypted at upload
- Cross Account access to bucket and objects


## Example: How to make S3 bucket public?
*(Slide 464)*

1. Block Public Access should be disabled
S3 Bucket Anonymous                                 internet Users


## Exercise: Public access to bucket
*(Slide 465)*

Bucket Policy
1. Block Public Access should be disabled
2. Bucket policy to allow access                     { "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": "*", "Action": "s3:GetObject",
S3 Bucket               "Resource": "arn:aws:s3:::EXAMPLE-BUCKET/*"
internet                       } Anonymous ] Users }


## Exercise: S3 bucket cross-account access
*(Slide 466)*

1. Bucket policy in Account B to allow access to Bucket Policy Account A
2. IAM user/role in Account A to allow UserA to    { access Bucket in Account B "Version": "2012-10-17", "Statement": [ { Account                            Account "Effect": "Allow", "Principal": " {"AWS":"arn:aws:iam::1111111111:root"}", "Action": "s3:GetObject", User A "Resource": "arn:aws:s3:::EXAMPLE- S3 Bucket BUCKET/*" } Account A                            Account B ] 1111111111                           2222222222 }


## Bucket Policy
*(Slide 467)*

- Restrict access from specific VPC
- Restrict access from specific VPC endpoint
- Restrict access from VPC source IP ranges VPC
- Restrict access from specific external IP
Client IP S3 Bucket
Internet


## Bucket Policy
*(Slide 468)*

- Restrict access from specific VPC
- Restrict access from specific VPC endpoint
- Restrict access from VPC source IP ranges
- Restrict access from specific external IP


## Bucket Policy
*(Slide 469)*

- Restrict access from specific VPC
- Restrict access from specific VPC endpoint
- Restrict access from VPC source IP ranges
- Restrict access from specific external IP


## Bucket Policy
*(Slide 470)*

- Restrict access from specific VPC
- Restrict access from specific VPC endpoint
- Restrict access from VPC source IP ranges
- Restrict access from specific external IP


## Bucket Policy
*(Slide 471)*

- Restrict access from specific VPC
- Restrict access from specific VPC endpoint
- Restrict access from VPC source IP ranges
- Restrict access from specific external IP


## S3 Data Security
*(Slide 472)*


## Data Security – Encryption in transit and rest
*(Slide 473)*

```text
                                                        Data in Transit                Data at Rest

                                        dfsdf#$#*(udksjahdla28sdfds#R#@@#@$@$2dffdsf



                                                                                       Encryption
```


## Certificate
*(Slide 474)*

```text
                                                                                              Authority (CA)
         HTTPS and SSL/TLS
                                              HTTPS Traffic


     https://somebank.com                                                                    somebank.com




                                                              https://youtu.be/cLYv4uSFJA8



                                                                               Public Key


                                                                                                  Private Key
```


## Encrypting data in transit – HTTPS (SSL/TLS)
*(Slide 475)*

- Amazon S3 provides endpoints
  - IPv4: s3.us-east-1.amazonaws.com
HTTPS/TLS
  - IPv4 and IPv6: s3.dualstack.us-east-1.amazonaws.com
  - FIPS IPv4: s3-fips.us-east-1.amazonaws.com
  - FIPS IPv4 and IPv6: s3-fips.dualstack.us-east-1.amazonaws.com
- It supports both HTTP and HTTPS transport layer protocols
- Recommended to use HTTPS as it uses TLS encryption for the data in transit


## Enforce HTTPS (SSL/TLS)
*(Slide 476)*

```text
                                              Bucket Policy




                                                                HTTP




                                                              HTTPS/TLS
```


## Securing data at Rest - Encryption
*(Slide 477)*

- S3 service encrypts object while saving it on disk and decrypts it when you
HTTPS/TLS Server-side
HTTPS/TLS access the object encryption                         •     Supports different Encryption keys SSE- S3, SSE-KMS, SSE-C
- Data is encrypted at the client side and then uploaded to S3 in the encrypted Client-side                              format. encryption                         •     S3 doesn’t know about the encryption and can not decrypt objects Server-side   Client-side encryption    encryption


## Server-side encryption
*(Slide 478)*

- SSE-S3 (Server-Side Encryption with Amazon S3-Managed Keys)
  - Enabled by default
HTTPS/TLS
  - Encryption keys are owned and managed by AWS
- SSE-KMS (Server-Side Encryption with KMS Keys)
  - AWS Key Management Store (KMS) to store the keys
- SSE-C (Server-Side Encryption with Customer-Provided Keys)
  - Customer owns and manage the encryption keys Server-side encryption


## Server-side encryption
*(Slide 479)*

SSE-S3                              SSE-KMS                                       SSE-C
KMS
- Encryption Keys managed by
- Encryption Keys managed by
- Encryption Keys managed by AWS S3 service                             AWS KMS                                     the Customer (outside of AWS)
- Enabled by default when you
- Useful if encryption key usage
- HTTPS must be enabled create new bucket or objects               needs to be audited by using              • Encryption key should be sent
- Should have request header:                CloudTrail logs                             in the HTTP header:
- Should have request header:               "x-amz-server-side-encryption-customer- "x-amz-server-side-encryption": "AES256" algorithm" : "AES256" "x-amz-server-side-encryption": “aws:kms" "x-amz-server-side-encryption-customer-key" : Base64,256bit encryption key


## Client-side encryption
*(Slide 480)*

- Client must encrypt the objects locally before sending it to S3 Client-side Encryption
- Client must also decrypt the objects locally after retrieving it from S3
- Amazon S3 has no role in encryption or decryption
- Use Amazon S3 Encryption Client to handle the client-side encryption
- You can additionally enabled S3 Server-Side Encryption
Amazon S3 Encryption Client


## SSE-KMS – important to know
*(Slide 481)*

- S3 Uses Envelope encryption for SSE-S3, SSE-KMS and SSE-C
- When using SSE-KMS, user must have IAM permissions
  - For PutObject: kms:GenerateDataKey
  - For GetObject: kms:Decrypt, kms:DescribeKey
- If you get "Access Denied (KMS)" when reading/writing S3 objects, the cause is usually missing kms:Decrypt or kms:GenerateDataKey in the KMS key policy.
- KMS keys are regional AWS Key Management System (KMS)
Envelope Encryption Create CMK kms:GenerateDataKey
PutObject                              Receives Plain text      Encrypt Request Data                                            Store Data Key +         Object using Key           encrypted Data key      Data Key


## SSE-KMS – important to know
*(Slide 482)*

KMS key rotation:
- It’s a good practice to enable the automatic KMS key rotation (default 1 year if ON)
- KMS key rotation creates new key material but Key ID, ARN etc. remains same. It also keeps older key material (version), so existing S3 objects remain decryptable.
- New uploads use the latest key version automatically, ensuring seamless rotation with no impact on applications.
KMS KeyID: 1234567890-abcde KeyID: 1234567890-abcde Material: 555666777888 Material: 111222333444                          Key rotation


## Enforcing encryption using Bucket policy
*(Slide 483)*

```text
                                              Must use SSE-S3 encryption
            {                                                                     Bucket Policy
                    "Version": "2012-10-17",
                    "Statement": [
                        {
                            "Sid": "DenyWhenNoSSES3Encryption",
                            "Effect": "Deny",
                            "Principal": "*",
                            "Action": "s3:PutObject",
                            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
                            "Condition": {
                                "StringNotEquals": {
                                    "s3:x-amz-server-side-encryption": "AES256"
                                }
                            }
                        }
                    ]
            }
```


## Enforcing encryption using Bucket policy
*(Slide 484)*

```text
                                              Must use SSE-KMS encryption
            {                                                                      Bucket Policy
                    "Version": "2012-10-17",
                    "Statement": [
                        {
                            "Sid": "DenyWhenNoSSEKMS",
                            "Effect": "Deny",
                            "Principal": "*",
                            "Action": "s3:PutObject",
                            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
                            "Condition": {
                                "StringNotEquals": {
                                    "s3:x-amz-server-side-encryption": "aws:kms"
                                }
                            }
                        }
                    ]
            }
```


## Enforcing encryption using Bucket policy
*(Slide 485)*

```text
                                              Must use specific SSE-KMS Key
            {                                                                             Bucket Policy
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Sid": "DenyWhenNoSSEKMSKey",
                        "Effect": "Deny",
                        "Principal": "*",
                        "Action": "s3:PutObject",
                        "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*",
                        "Condition": {
                            "ArnNotEqualsIfExists": {
                                "s3:x-amz-server-side-encryption-aws-kms-key-id": "arn:aws:kms:us-
            east-1:111122223333:key/01234567-89ab-cdef-0123-45abc"
                            }
                        }
                    }
                ]
            }
```


## Amazon Macie
*(Slide 486)*

- Fully managed S3 data security service
- Can discover Personally Identifiable Information (PII) data
- Uses machine learning and pattern matching to discover, monitor, and protect your data in S3
- Triggers alert when sensitive data is identified
Analyse                  Notify                 Integrations
S3 Bucket                 Amazon Macie             Amazon                      Lambda     Classification   Data Security EventBridge                  Function     Channel           Team


## S3 Versioning
*(Slide 487)*


## Amazon S3 bucket versioning
*(Slide 488)*

- Creates a new version with every upload of the object. PUT Request
- Previous versions are not overwritten.
- Should specify version ID for deleting the object version.                     Key = tax.pdf
- Delete requests without a version ID removes access to objects (error 404) but keeps the data. There is a delete marker added as a new version.
- You can transition or expire current and non-current                                                   Key = tax.pdf Version ID = 2 versions with S3 lifecycle rules Key = tax.pdf
- There will be cost for storing all the versions.                                                       Version ID = 1
- Must enable bucket versioning for using Bucket                                                            Bucket replication, Object lock and MFA delete features
Use versioning to protect your data from accidental deletion or for recovering / rollback to the older versions of the objects


## Exercise: Enable S3 Versioning
*(Slide 489)*

```text
             1    Create S3 Bucket.

             2    Go to Bucket -> Properties -> Versioning -> Enable

             3    Upload sample text file to S3 Bucket.

             4    Upload modified text file with same name multiple times by doing small changes every time.

             5    Check Versions. On S3 Console -> Bucket -> Versions -> Show. How many versions do you see?

             6    Try deleting a particular Version of the uploaded file.

             7    Click Hide Version. S3 Console -> Bucket -> Versions -> Hide Versions (You should see only one file)

             8    Delete a File. Deleted?

             9    Click Show Version. S3 console -> Versions -> Show versions. Restore file using desired version.
```


## Object Lock
*(Slide 490)*

- S3 Object Lock helps prevent S3 objects from being deleted or overwritten for a fixed amount of time or indefinitely
- Useful to meet WORM (Write Once Read Many) storage regulatory compliances required in Finance, Insurance and healthcare etc. industries. Versioning
- S3 bucket Versioning must be enabled for using Object lock feature
- Object Retention – How long and under which conditions object should be retained
  - Retention Mode
    - Compliance Mode – Even root users cannot delete/alter objects until retention expires.
    - Governance Mode – Protected unless special permissions allow override s3:BypassGovernanceRetention
  - Retention Period
    - Fixed period during which Object remains locked                                                     S3 Bucket
    - Can set default retention period at the S3 bucket level or unique retention period at individual object level
  - Legal Hold
    - No expiration date
    - Remains in place until explicitly removed at object version using permissions s3:PutObjectLegalHold


## MFA Delete
*(Slide 491)*

- Helps prevent accidental or malicious deletions by adding second layer of protection using Multi- Factor Authentication (MFA)
- Requires MFA code for:
  - Deleting object versions
  - Changing state of S3 bucket’s versioning
- S3 Bucket versioning must be enabled
- Must use AWS CLI, SDK or APIs to enable MFA delete
- Can be enabled/disabled only by the Root user.
- Protection applies at the bucket level, not per-object.                     Versioning
- Works independently from S3 Object Lock (can be used together).


## To enable MFA Delete
*(Slide 492)*

- Generate an access key and secret key for the root user.
- Activate an MFA device for the root user.
- Configure the AWS CLI with the root user credentials.
- Configure MFA delete by running CLI command:
aws s3api put-bucket-versioning --bucket amzn-s3-demo-bucket-1 --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "SerialNumber 123456"
Confirm that MFA delete is working and delete your root access keys.


## S3 Lifecycle
*(Slide 493)*

  - Helps store objects cost effectively throughout their lifecycle by transitioning them to lower-cost storage classes, or deleting objects
- S3 follows waterfall model for lifecycle
- Transition Action
    - Transitions objects to different storage class
    - Example: Move objects from S3 Standard class to S3 Standard-IA class after 30 days
- Expiration Action
    - Delete objects / versions after specified period
    - Delete incomplete multi-part uploads
Expiration Rules


## S3 Lifecycle Rules
*(Slide 494)*

- Transition minimum age - The minimum number of days an object must remain in its current storage class before a lifecycle rule can transition it to a colder storage class
  - Objects must be stored for at least 30 days before transitioning to S3 Standard-IA or S3 One Zone-IA
  - Similarly, S3 Glacier Instant Retrieval has a minimum storage duration of 90 days, and Glacier Deep Archive has a minimum storage duration of 180 days
- Lifecycle rules can be applied to specific prefixes, object tags or object size
  - Prefixes: logs/2025/november/*
  - Tag: environment=development
  - Size: Define minimum and maximum e.g. between 1000MB and 5000MB
- Objects smaller than 128 KB will not transition by default to any storage class
- Lifecycle processing is asynchronous (does not do it immediately)
- Can be applied within a bucket. Can not be used for cross-bucket transfers.


## S3 Lifecycle Rules
*(Slide 495)*


## S3 Lifecycle vs S3 Intelligent Tiering
*(Slide 496)*

S3 Lifecycle                S3 Intelligent Tiering
- User-defined rules move objects between
- Automatic tier changes based on object access storage classes                                 patterns
- Have to specify the duration (days) for the
- Can not specify duration. Transition is based on transition                                      access pattern.
- Follows waterfall model, one-directional
- Can move object both ways
- Supports Expiration Rules
- Does not support Expiration/Deletion rules
- Use case: Known access pattern
- Use case: Unknown access pattern
- Examples: Logs, Backups, Archives
- Example: Data Lake, Analytics, Media or Application data


## Scenario
*(Slide 497)*

```text
         A News media company stores full news articles uploaded by editors into Amazon S3. These articles are
         frequently accessed for the first 30 days while they remain trending. After 30 days, they are rarely
         viewed but must be retained for archival/legal compliance for 7 years. If readers access older articles,
         they are fine waiting up to 12 hours for retrieval. A short summary snippet is also generated for each
         article for homepage display, and these snippets will be accessed primarily for first 30 days after which
         they can be accessed rarely for up to maximum 90 days.
         How would you design this?

                                                       Frequently         Infrequently
                                                       accessed            accessed




                                                        Frequently
                                                        accessed                                   Rarely accessed


                                                                                             Can wait for up to 12 hrs

                                                                                                                                   time
                                              0 days                 30 days             90 days                         7 years
```


## Scenario
*(Slide 498)*

For Original News Articles
- Store new articles in S3 Standard for fast access during the first 30 days.
- Add an S3 lifecycle transition rule:
  - Transition articles to S3 Glacier Deep Archive after 30 days (cheapest tier, 12-hour retrieval acceptable).
- Add an S3 lifecycle Expiration rule:
  - Expire/delete articles from S3 Glacier Deep Archive after 7 years
- (Optionally) Add Object lock
  - Compliance mode for 7 years duration
Object Lock Frequently accessed                                     Rarely accessed
Can wait for up to 12 hrs S3 Standard                S3 Glacier Deep Archive time 0 days                       30 days         90 days                         7 years


## Scenario
*(Slide 499)*

For Summary files
- Store Summary files in S3 Standard for first 30 days.
- Add an S3 Lifecycle rule to move summary files to S3 One Zone-IA after 30 days. (Note that summary files can be re-created and hence do not need to be stored in S3 standard-IA tier)
- Add an S3 lifecycle Expiration rule to delete summary files after 90 days
S3 Standard                S3 One Zone-IA
Frequently accessed                                Rarely accessed
Can wait for up to 12 hrs S3 Standard time 0 days                     30 days      90 days                         7 years


## S3 Replication
*(Slide 500)*

SRR
- Automatic and asynchronous replication of objects between S3 buckets
- Bucket versioning must be enabled on both source and destination buckets                                                                        Source Destination
- Supports - Same Region Replication (SRR) & Cross Region Replication (CRR)
- Supports Live Replication for new objects and On-demand S3 Batch Destination Replication for existing objects
- (Optional) S3 Replication Time Control (RTC) for 15min SLA S3 Replication use cases:                                                               CRR
  - Minimize latency by brining data closer to the end user (CRR)
  - Aggregate logs into centralized bucket (SRR) Destination
  - Live replication from Production to Test environment (SRR)
  - Data Sovereignty – Copies of data in different accounts
  - Compliance – store copy of data at distance (CRR)               Source
Destination


## S3 Replication – Important to know
*(Slide 501)*

- Cross-Region Replication incurs data transfer cost
- For encrypted objects (e.g. SSE-KMS) the appropriate IAM permissions for the KMS are required depending on SRR and CRR.
- You can use filters (object prefix, tags) to narrow down the scope for objects to be replicated
- Replication chaining is not supported (A -> B -> C). You can set up a separate rule for A -> C replication or use Batch replication for B -> C.
- Replication Time Control (RTC) offers 15-minute SLA replication ensuring 99.99% of new objects are replicated to the destination bucket within 15 minutes.
- Replication status can be monitored using CloudWatch Metrics:
  - Metrics: Bytes pending replication, Replication latency, Operations Pending/Failed Replication
  - You can receive the failed replication status using S3 Event Notification


## How delete operations affect replication?
*(Slide 502)*

When deleting Objects without using Version ID
- When Object is deleted in the source bucket, DELETE marker is created in the source bucket
- This DELETE marker is not replicated to the target bucket
- Optionally, we can enable DELETE marker replication at the bucket level or object level
- After enabling, the DELETE marker is also replicated, and object is considered to be deleted in the target bucket
When deleting Objects using Version ID
  - Object is deleted in the source bucket but its not deleted in the destination bucket
  - This protects data from malicious deletions


## Exercise - S3 Replication
*(Slide 503)*

```text
                                                            1   Create source and destination buckets

                                                            2   Enable versioning for both the buckets

                                                            3   In source bucket, go to Management and create
                                                                Replication rule

                                                            4   Upload an object into source bucket and wait for few
                 Source                       Destination       minutes

                                                            5   Verify in the target bucket if you see the same object
                                                                with same object version
```


## S3 Event Notification
*(Slide 504)*

- Notifies the target service when certain S3 event occurs
- Supported notification destinations:
  - Amazon SNS (SNS:Publish)
  - Amazon SQS (SQS:SendMessage)                                          SNS
  - AWS Lambda (lambda:InvokeFunction)
- S3 Events:
  - s3:ObjectCreated -> New object added SQS
  - s3:ObjectRemoved -> Object deleted                              S3
  - s3:ObjectRestore:* -> Glacier restore initiated or completed
  - s3:Replication:* -> Replication success/failure status events
  - s3:Lifecycle* -> Lifecycle transition and expiration events Lambda
  - s3:IntelligentTiering -> Intelligent Tiering archive events


## S3 Event Notification
*(Slide 505)*

- You can filter the notifications by Object prefix (/images/*) or suffix (.jpg)
- If versioning is enabled, notifications are sent for each version
- Delivery – at-least-once (ordering is not guaranteed) SNS
- Notification contains object metadata, not the object data.
SQS S3
Lambda


## Resource based policy to allow S3 send event notification
*(Slide 506)*

```text
            {                                                                      Lambda Function Policy
                "Version": "2012-10-17",
                "Statement": [
                  {
                    "Sid": "AllowS3Invoke",
                    "Effect": "Allow",
                    "Principal": {
                       "Service": "s3.amazonaws.com"
                    },
                    "Action": "lambda:InvokeFunction",
                    "Resource": "arn:aws:lambda:<REGION>:<ACCOUNT-ID>:function:<FUNCTION-NAME>",
                    "Condition": {
                       "ArnLike": {
                         "AWS:SourceArn": "arn:aws:s3:::<BUCKET-NAME>"
                       }
                    }
                  }
                ]
            }
```


## Exercise - S3 Event Notification
*(Slide 507)*

```text
                                                    1   Create SNS topic and create email subscription

                                                    2   SNS Topic Access Policy to allow S3 to send
                                                        notification

                                                    3   Check your email and confirm the subscription

                 Source                       SNS   4   Create S3 event notification with SNS as target

                                                    5   Upload an object into source bucket and wait for few
                                                        seconds

                                                    6   Verify if you have received an email
```


## SNS Topic - Sample Access policy
*(Slide 508)*

```text
            {                                                                       SNS Access Policy
                "Version": "2012-10-17",
                "Statement": [
                  {
                    "Sid": "AllowS3ToPublish",
                    "Effect": "Allow",
                    "Principal": { "Service": "s3.amazonaws.com" },
                    "Action": "SNS:Publish",
                    "Resource": "arn:aws:sns:<region>:<account-id>:<topic-name>",
                    "Condition": {
                      "ArnLike": {
                        "aws:SourceArn": "arn:aws:s3:::<bucket-name>"
                      }
                    }
                  }
                ]
            }
```


## S3 to Amazon EventBridge
*(Slide 509)*

- All S3 API events are automatically sent to Amazon EventBridge (source: aws.s3)
- EventBridge can trigger 25+ targets including Step
  - Step Functions                                                                               Functions
  - ECS tasks Filtering &
  - SQS                                                     All                 transformation
  - Lambda functions
  - API destinations                                  S3           Amazon
  - Custom applications                                           EventBridge
- Use EventBridge when:
  - You need multiple downstream targets (e.g. Kinesis Data streams, SQS)                        Lambda
  - Cross-account event routing
  - Advanced filtering rules (based on event fields in JSON, object metadata, size)                Other
  - Need to trigger a workflow (e.g. Step Functions)                                              targets


## S3 events - Exam Scenarios
*(Slide 510)*

1. Scenario: Generate an image thumbnail immediately whenever a user uploads a profile picture into the images/ folder. => Use S3 Event Notification to Lambda where lambda can process the image and create thumbnails
2. Scenario: A compliance team wants to audit every S3 API call, including PUT, DELETE, tagging changes, access point usage, and internal S3 events, and route them to a central audit account. => Use Amazon EventBridge to route all the S3 API calls to centralized Audit account
3. Scenario: A video-processing pipeline needs to queue uploaded videos for background processing using a consumer fleet that scales automatically. => Use Event Notification to Simple Queue Service (SQS) where video (object) metadata e.g. path, date, size etc. is sent to SQS queue. These messages in the queue are processed asynchrounsly by the downstream applications like Lambda or ECS tasks.


## S3 events - Exam Scenarios
*(Slide 511)*

4. Scenario: A news app needs to fan-out notifications (email, SMS, mobile push) to multiple subscribers as soon as a new article is uploaded. => Use S3 Event Notification to Simple Notification Service (SNS) topic. Topic should be subscribed by the end users / mobile
5. Scenario: A finance team wants to trigger monthly billing workflows across multiple AWS accounts whenever a .csv report lands in a reports/ folder => Use Amazon EventBridge to trigger AWS Step function workflow across multiple AWS accounts.
6. Scenario: A media archive system wants to process archived files automatically using Lambda when a Glacier restore completes => Use S3 Event Notification to Lambda when Glacier restore event is generated


## S3 Server Access Logs
*(Slide 512)*

- S3 Server access logging provides detailed records for the requests made to a bucket
- Log record includes details like requester, bucket name, object key, operation, response status, error code, bytes transferred etc.
Access
- Logs are delivered to another S3 bucket (can not be the same bucket as source)
- Not real-time, may take minutes to hours
- No additional charge for enabling Access logs but pay for logs storage.
- May generate huge logs, recommended to use S3 Lifecycle to move logs to cheaper                  Access logs storage and purge after some time
- Use cases:
  - Security / Audit                                                              Source            Logging Bucket            Bucket
  - Access pattern analysis
  - Cost optimization by finding highly accessed objects
- Never use the same bucket as source and target for Access logging


## S3 Inventory
*(Slide 513)*

- Provides a scheduled report (daily or weekly) listing all objects and metadata in a bucket.
- Includes details like object size, storage class, encryption status, replication status, object version, ETag, last modified date, and Object Lock info etc.
- Often used with Amazon Athena for large-scale analysis or auditing for encryption and replication compliance. Inventory
- Cost and performance efficient than using S3 List API operations which may otherwise scan millions of the objects for large size buckets                       Source                  Inventory Bucket                   reports
- Inventory scope may be configured by object prefix, current version or all versions Analyse
Tip: Need a scalable, periodic report of all objects and their metadata → Use S3 Inventory (not ListObjects API). Athena


## IAM Access Analyzer for S3
*(Slide 514)*

AWS Account
AWS IAM User
- Analyses the S3 ACL and Bucket policies to ensure that only required entities have access to S3 buckets AWS IAM Role
- Identifies Publicly accessible S3 buckets                     S3 Bucket
- Identifies buckets which are shared with other AWS accounts Public Access
Federated User Anonymous User


## S3 Storage Lens
*(Slide 515)*

- It’s a visualization tool which provides organization-wide visibility into S3 storage usage and activity across all buckets, Regions, and accounts (can include AWS Organizations).
- Offers dashboard-based analytics with over 100+ metrics, including storage usage, object counts, versions, encryption status, activity trends, and replication metrics.
- Helps identify cost optimization opportunities such as unused buckets, non-IA- eligible data, incomplete multipart uploads, and objects without encryption.
- Supports advanced metrics & recommendations (paid tier) including per-prefix            S3 Storage Lens insights, activity metrics (PUT/GET/delete rates), and security findings.
Tip: Need AWS organization-wide S3 usage & activity analytics → Use S3 Storage Lens


## S3 Storage Lens
*(Slide 516)*

*Source: https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage_lens.html*


## S3 Monitoring, Logging and Reporting - Summary
*(Slide 517)*

*Source: ChatGPT*


## Amazon S3 Performance optimization
*(Slide 518)*

- Prefix partitioning - GET/PUT transaction optimization
- Multi-part Upload and byte-range fetches - Data upload and retrieval optimization
- S3 Transfer Acceleration - Long distance network latency optimization
- Amazon CloudFront - Frequently accessed data optimization using Cache


## Amazon S3 – Prefix partitioning
*(Slide 519)*

- S3 automatically scales to high request rates by partitioning data based on prefixes.                                                 /logs/2024_01_05_app.log
- You can achieve at least:
  - 3,500 PUT/COPY/POST/DELETE requests/sec per prefix /logs/2024/01/05/app.log
  - 5,500 GET/HEAD requests/sec per prefix Example 1
- To increase throughput, use multiple prefixes so S3 can create more partitions. images/hash-01/img123.jpg
- Avoid creating millions of objects under a single “hot” prefix      images/hash-02/img124.jpg (e.g., logs/ only).                                                 images/hash-03/img125.jpg Example 2
- S3 scales horizontally, so the more parallel requests you make, the higher the throughput. Use parallelize operations especially for analytics workloads                                iot/device-01/2024/01/06/data.json
- Prefixing also helps with efficient batch operations, list        iot/device-02/2024/01/06/data.json iot/device-03/2024/01/06/data.json patterns, partitioned queries, and lifecycle management. Example 3


## Multi-part upload
*(Slide 520)*

- Recommended for objects >100 MB;
- Must for objects > 5 GB
- If a part fails, only that part is retried - faster recovery and better resilience.
- Use Multipart Upload + Parallel Threads to utilize maximum network bandwidth for the upload.
Large Object > 100MB
Parallel     Reconstruct Split in parts   upload parts     Object


## Byte-range fetch
*(Slide 521)*

- Allows downloading different byte ranges of the same                             File in S3 object in parallel.                                          0   100     200    300   400   500   600   700   800
- S3 supports extremely high read throughput when doing multiple range GETs.
- Great for:                                                             Download all parts in parallel
  - Media streaming
  - Large file downloads 0   100      200   300   400   500   600   700   800
  - ETL workloads needing partial reads
  - Resumable downloads
Example (using AWS CLI):                                        Download specific range bytes
$aws s3api get-object --bucket amzn-s3-demo-bucket1 --key folder/my_data --range bytes=0-500 my_data_range.output


## S3 Transfer Acceleration
*(Slide 522)*


## AWS Edge locations
*(Slide 523)*

*AWS Edge locations*

**Diagram just for illustration, not actual*


## Without AWS edge network
*(Slide 524)*

```text
                                     App




         AWS Edge locations

        Non-AWS network

        AWS region

                                              *Diagram just for illustration, not actual
```


## With AWS edge network
*(Slide 525)*

```text
                                     App




         AWS Edge locations

        Non-AWS network

        AWS region

             AWS backbone network

                                              *Diagram just for illustration, not actual
```


## AWS edge network and services
*(Slide 526)*

AWS Edge locations are used by different AWS services to lower the network latency for end users
- Use AWS edge network by using:                                                                                 Region Local ISP Network      Network     Network   Network    Network
  - Amazon CloudFront                             Hop          Hop         Hop       Hop        Hop
  - AWS Global Accelerator        End user
  - AWS S3 Transfer Without AWS edge network Acceleration Region Local ISP    Edge                AWS Backbone network Location
End user
With AWS edge network


## S3 Transfer Acceleration (S3TA)
*(Slide 527)*

- Speeds up content transfer to and from S3 bucket by 50-500% for long-distance transfer of larger objects.
- Routes traffic through globally distributed Edge Locations and over AWS backbone networks
- Additionally, uses network protocol optimizations
- We can enable/disable S3 Acceleration for S3 bucket
- Provides new s3 bucket endpoint when accelerator is enabled e.g. bucket-name.s3- accelerate.amazonaws.com
AWS Backbone network
User in Japan                  Edge Location                                                 Bucket in N. Virginia (S3TA enabled)
AWS Tool: https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html


## Content caching using Amazon CloudFront
*(Slide 528)*

- Amazon CloudFront is a Content Delivery Network (CDN) service.​
- It supports many different origins including S3 Bucket
- CloudFront caches the static contents e.g. images, videos etc. at the edge locations
- Reduced Data transfer out rate as compared to internet (+1TB free data transfer out per month)​​
Cached
AWS Backbone network
Amazon                                            S3 bucket CloudFront                                   (as CloudFront Origin)


## Block direct access to S3
*(Slide 529)*

CloudFront Origin Access Identity (OAI)                    CloudFront Origin Access Control (OAC)
- Legacy mechanism to access a
- New mechanism to access a private private S3 bucket.                                     S3 bucket.
- Creates IAM-like identity
- Uses SigV4 signing
{                                                      { "Version": "2012-10-17",                               "Version": "2012-10-17", "Statement": [                                         "Statement": [ {                                                      { "Sid": "AllowCloudFrontAccess",                        "Sid": "AllowCloudFrontOAC", "Effect": "Allow",                                     "Effect": "Allow", "Principal": {                                         "Principal": { "AWS":                                                 "Service": "cloudfront.amazonaws.com" "arn:aws:iam::cloudfront:user/CloudFront Origin              }, Access Identity <OAI-ID>"                                    "Action": "s3:GetObject", },                                                     "Resource": "arn:aws:s3:::<bucket-name>/*", "Action": [                                            "Condition": { "s3:GetObject"                                         "StringEquals": { ],                                                         "AWS:SourceArn": "Resource": "arn:aws:s3:::<bucket-name>/*"       "arn:aws:cloudfront::<account- }                                                  id>:distribution/<distribution-id>" S3 bucket ]                                                            }                                           (as CloudFront Origin) }                                                            } } ] }


## S3 Batch Operations
*(Slide 530)*


## S3 Batch Operations
*(Slide 531)*

- Allows to perform bulk operations on millions or billions of S3 objects in a single managed job. Select
- Operates on a manifest file, which is a list of object keys generated by:                Objects
  - S3 Inventory file or custom CSV file
  - Generate objects list by specifying source and filter (New option)
- Supports multiple actions: Select an
  - Copy objects across buckets or within same bucket (can also encrypt during    operation copy)
  - Replace or Delete object tags
  - Modify ACLs
  - Restore Glacier objects Start the job
  - Initiate S3 object lock retention changes
  - Execute AWS Lambda functions on each object                                  Monitor the progress
- Entire job is fully managed by S3 - includes retry logic, progress tracking, and detailed status reporting.
- Use cases: Bulk updates or processing, Large migrations, Metadata cleanup, Object lock changes, Restoring objects from Archive                                      Output


## Demo – Replace Object tags
*(Slide 532)*

```text
         1    Create a csv manifest file with list of objects and upload to your S3 bucket


         2    Create IAM role with permissions and trust policy as defined here
              https://docs.aws.amazon.com/AmazonS3/latest/userguide/batch-ops-iam-role-policies.html


         3    Create S3 Batch Operations Jobs – Provide location for the manifest file and select Action as
              Replace Tags. Provide the Tags to be added. Optionally provide the details to save the job
              execution results.

         4    Run the job and wait to finish


         5    Verify whether tags are added for the objects listed in the manifest files
```


## S3 Batch Operations - Demo
*(Slide 533)*

- Allows to perform bulk operations on millions or billions of S3 objects in a single managed job.
- Operates on a manifest file, which is a list of object keys generated by:
  - S3 Inventory file or custom CSV file
  - Generate objects list by specifying source and filter (New option)
- Supports multiple actions:
  - Copy objects across buckets
  - Replace or Delete object tags
  - Modify ACLs
  - Restore Glacier objects
  - Initiate S3 object lock retention changes
  - Execute AWS Lambda functions on each object
- Entire job is fully managed by S3 - includes retry logic, progress tracking, and detailed status reporting.
- Use cases: Bulk updates or processing, Large migrations, Metadata cleanup, Object lock changes, Restoring objects from Archive


## Static Website Hosting
*(Slide 534)*

- Because S3 can be accessed over the Web, we can host Static (http) website on S3
- For Setting up Static Website on S3:
1. Upload static HTML files to S3 bucket
2. Make S3 bucket Public (Disable Block Public Access & set Bucket Policy)
3. Enable Static website hosting for the bucket
- Depending on your Region, your Amazon S3 website endpoint follows one of these two formats.
  - s3-website dash (-) Region ‐ http://bucket-name.s3-website-Region.amazonaws.com
  - s3-website dot (.) Region ‐ http://bucket-name.s3-website.Region.amazonaws.com
Mumbai http://example.com.s3-website.ap-south-1.amazonaws.com
User (internet)                                                     example.com


## Exercise: Static Website Hosting
*(Slide 535)*

1                 Create S3 Bucket in the region of your choice.
2                 Disable “Block Public Access” setting for the bucket
3                 Download sample static website template and extract locally on your machine.
4 Upload static website content to S3 bucket:
- Drag and drop all files and folders directly in the bucket.
- Make sure index.html file is directly inside bucket and not inside folder.
5                 Enable Static Website for the bucket:
- Select Bucket -> Properties -> Static Website Hosting -> Use this bucket to host a website.
- Provide index document as “index.html” -> Save
6                 Try to access website using HTTP endpoint. You should get Permissions denied error. <bucket-name>.s3-website.<AWS-region>.amazonaws.com


## Exercise: Static Website Hosting
*(Slide 536)*

```text
            7           Go to bucket permissions and add following Bucket Policy. This allows public READ access to
                        all objects in your bucket:
                            {
                                "Version": "2012-10-17",
                                "Statement": [
                                  {
                                     "Sid": "PublicReadGetObject",
                                     "Effect": "Allow",
                                     "Principal": "*",
                                     "Action": "s3:GetObject",
                                     "Resource": "arn:aws:s3:::<your bucket name>/*"
                                  }
                                ]
                            }

            8           Now access website using following URL. You should be able to access it.

                        <bucket-name>.s3-website.<AWS-region>.amazonaws.com
```


## Requester Pays                                                                              Requester AWS Account
*(Slide 537)*

- A Feature that can be enabled for S3 Buckets
- Moves data access costs (GET, HEAD, LIST requests) and data transfer cost (DTO) from the bucket owner to the requester.
- Bucket owner still pays for S3 storage cost
Request Request
- Use cases:
  - When you host public datasets or large shared files where many external users download content.
  - Often used by organizations sharing research data, logs, public archives, or community datasets.
  - Helps prevent unwanted or extremely high egress charges for open-access datasets.
- Requesters must use:
  - Header: x-amz-request-payer: requester (when using APIs)                                          Requester Pays
  - Parameter: --request-payer requester (when using CLI)       Owner AWS Account                        Enabled
- Works only when requesters have AWS accounts.                                             DTO = Data Transfer Out charges


## S3 Pre-signed URLs
*(Slide 538)*

- Allows temporary access to an S3 object without giving users AWS                               Bucket Owner credentials.
- A pre-signed URL is generated using creators’ IAM credentials. If the                                                        1 creator cannot access/upload object, the URL will not work.. 2
- Supports GET (download) and PUT (upload) operations
- Can be generated using AWS console or AWS CLI                                                           3 Share URL
- Use cases:
  - User wants to download an invoice PDF from a private S3 bucket.
  - A mobile app uploads profile pictures directly to S3.
  - A course instructor shares a file with students using a link that expires in 1 4 hour. Upload / Download using URL
- Important considerations:
  - Anyone with the URL can access the object until the URL expires
  - If the IAM principal loses permissions after generating the URL, URL                  Anonymous continues to work until expiry.                                                       User / Client


## CORS (Cross-Origin Resource sharing)
*(Slide 539)*

- CORS (Cross-Origin Resource Sharing) is a browser security mechanism that                          myapp.com   Browser controls which websites are allowed to make requests to your domain.
- Browsers enforce a rule called Same-Origin Policy.
- A "cross-origin" request means a web page from one domain tries to access
mybucket.s3.amazonaws.com resources from another domain.
  - Example: Website https://myapp.com calling https://mybucket.s3.amazonaws.com
- Flow of the events:
1. If browser see cross-origin request, it first sends Pre-flight (HTTP OPTIONS) request to check whether it is allowed to make cross-origin request to S3 bucket
2. S3 checks CORS rules in the bucket to check if Origin and HTTP Method is allowed
3. S3 returns CORS response
4. If allowed, browser then sends actual request (GET/PUT/DELETE etc.)


## How it works?
*(Slide 540)*

```text
                                              OPTIONS /myobject
                                              Origin: https://myapp.com
                                              Access-Control-Request-Method: PUT
                                              Access-Control-Request-Headers: Content-Type

                                                    Preflight Request (HTTP OPTIONS)


                                                              CORS Response
            Browser
                                              Access-Control-Allow-Origin: https://myapp.com
                                              Access-Control-Allow-Methods: PUT, GET
                                              Access-Control-Allow-Headers: Content-Type



                                                            Actual Request                     S3 Bucket



    Error: Blocked by CORS
```


## CORS with S3
*(Slide 541)*

Browser
- S3 Bucket should allow correct CORS including Origin name, methods and                                myapp.com headers
- We can use * as a domain name to allow all origins
- Sample S3 CORS configuration
mybucket.s3.amazonaws.com <CORSConfiguration> <CORSRule> <AllowedOrigin>https://myapp.com</AllowedOrigin> <AllowedMethod>GET</AllowedMethod> <AllowedMethod>PUT</AllowedMethod> <AllowedHeader>*</AllowedHeader> </CORSRule> </CORSConfiguration>
Note:
  - CORS is browser only feature and hence not applicable when accessing S3 using cURL, postman, CLI, SDK, Lambda, APIs etc.
  - Applicable when using Java Script, Presigned URL from browser, Accesing S3 static website etc.


## S3 Access Points                                                      Same Account
*(Slide 542)*

- S3 Access point makes it simple to manage access at scale for applications using shared datasets
- Using Bucket policy for managing access to thousands of application with different level of access is complex           Cross-account
- Create multiple Access points per S3 bucket
  - Customized path into a bucket (prefix)
  - Unique hostname per Access Point
  - Access policy per Access point
  - Network controls (Internet or VPC) Multiple apps requiring access
Shared data sets


## S3 Access Points
*(Slide 543)*

```text
                                              Request for data from S3



                                                                S3 Access point with Access point   Shared S3 bucket
                                                                             policy
```


## S3 Access Points - Example
*(Slide 544)*

```text
                                                            Example Access point DNS

         finanace-123456789012.s3-accesspoint.us-east-1.amazonaws.com         sales-123456789012.s3-accesspoint.us-east-1.amazonaws.com
         datascience-123456789012.s3-accesspoint.us-east-1.amazonaws.com




                                                                                              Example Access point ARN

                                                                            arn:aws:s3:us-east-1:123456789012:accesspoint/finance
                                                                            arn:aws:s3:us-east-1:123456789012:accesspoint/sales
                                                                            arn:aws:s3:us-east-1:123456789012:accesspoint/datascience
```


## S3 Access Points – Example policy
*(Slide 545)*

```text
             {
                 "Version": "2012-10-17",
                 "Statement": [
                   {
                     "Sid": "AllowFinanceTeamRead",
                     "Effect": "Allow",
                     "Principal": {
                        "AWS": "arn:aws:iam::123456789012:role/FinanceTeamRole"
                     },
                     "Action": [
                        "s3:GetObject"
                     ],
                     "Resource": "arn:aws:s3:us-east-1:123456789012:accesspoint/finance/object/*"
                   }
                 ]
             }
```


## VPC only Access point
*(Slide 546)*

- You can configure an S3 Access Point so it is reachable only from a specific          Virtual private cloud VPC.                                             (VPC)
- A VPC Endpoint (Gateway or Interface) is required for applications in that VPC to access the Access Point.
- The VPC Endpoint Policy must explicitly allow access to both the S3       Client VPC Endpoint VPC Access Point Shared S3 bucket and the Access Point ARN.                                                                bucket


## VPC only Access point – VPC endpoint policy
*(Slide 547)*

```text
            {
                "Version": "2012-10-17",
                "Statement": [
                  {
                    "Sid": "AllowAccessToBucketAndAccessPoint",
                    "Effect": "Allow",
                    "Principal": {
                      "AWS": "arn:aws:iam::<account-id>:role/<iam-role-or-user>"
                    },
                    "Action": [
                      "s3:GetObject",
                      "s3:ListBucket"
                    ],
                    "Resource": [
                      "arn:aws:s3:::<bucket-name>",
                      "arn:aws:s3:::<bucket-name>/*",
                      "arn:aws:s3:<region>:<account-id>:accesspoint/<access-point-name>",
                      "arn:aws:s3:<region>:<account-id>:accesspoint/<access-point-name>/object/*"
                    ]
                  }
                ]
            }
```


## AWS Container
*(Slide 548)*

*Amazon ECS, Amazon EKS and AWS Fargate*

*services*


## AWS compute choices
*(Slide 549)*

```text
         Virtual Machines                               Containers                       Functions


                                                                                             Serverless



                     EC2                      Elastic Container   Elastic Kubernetes    Lambda
                                               Service (ECS)        Service (EKS)

                                                           Data plane
                                                                           Serverless



                                                   EC2               Fargate
```


# AWS Container Services

*(Source: Slide 550)*

### Containers


## Why containers?
*(Slide 551)*

```text
                                                 Libc 5                 Libc 5
                             Code
                                                 Glib 2.81.0            Glib 2.81.0
                                                 OpenSSL 3.2.0          OpenSSL 3.1.0
                             Libs/dependencies   +more                  +more

                           Configurations




                                                                 Push



                   Developer’s                                           Server
                   workstation
```


## Why containers?
*(Slide 552)*

```text
                               Code

                            Libs/dependencies

                           Configurations




                                                Push



                   Developer’s                         Server
                   workstation
```


## Containers
*(Slide 553)*

```text
        Lightweight virtualization that allows to run applications and their dependencies in resource-isolated
        processes



                                                                                      App              App
        Key benefits
                                                                                   Bin/Library     Bin/Library
         Isolated - Isolated filesystem, process space, network,                    Container
                                                                                                   Virtual
                                                                                                     Container
         environment
         Lightweight - No full OS overhead, minimal kernel, fast                     Container Runtime
         Consistency – Contains all dependencies, same everywhere
                                                                                      Operating System
         Portability – Can be easily packaged, shipped and run on any
         platform (local machine, server, cloud)


                                                                                            Hardware
```


## What makes containers work?                                                   Container Engines
*(Slide 554)*

```text
                                       Provide isolation for running applications by
                                       limiting what they can see and access.

      Namespaces


                                       Limit and monitor the resource usage
                                       (CPU, memory, I/O) of containers.
     Control Groups
       (cgroups)


                                     Enables the layering of filesystems, allowing
                                     containers to share common files and
                                     efficiently manage disk space.


     Union Filesystem
```


## What is docker?
*(Slide 555)*

*Docker is an open platform for developing, shipping, and running applications as containers*


## Run anywhere
*(Slide 556)*

```text
                                                                Developer’s
                                                                 machine




                                                                 Physical
                                                                  Server

                         Container
                          Image


                                                                  Cloud
                                              Virtual Machine
```


## How to use docker?
*(Slide 557)*

```text
                             1                                     2                                           3

                 Write docker file                          Build Container                             Run container from
                                                                 Image                                      the image

                                                                                     Image repository




                                                                 container
                                       > docker build -t                                > docker run
                                                                   image

                                                                   image

            FROM ubuntu:18.04
                                                                                                           Container
                                                                 base image
            COPY . /app
            RUN make /app
            CMD python /app/app.py
                                                                  Kernel (Host OS)

                                                           Docker image
```


## Docker Image
*(Slide 558)*

```text
                                                       Packed Application code
                                                       & dependencies

                                                       Repeatable

                                                       Immutable
                                                                                      Docker Hub
                                                       Portable
                                                                                   (Container Registry)
                                   container

                                     image
                                                                          push
                                     image

                                  base image

                                                                                         Amazon ECR
                                    Kernel (Host OS)                             (Elastic Container Registry)
```


## Exercise – Run docker container on EC2 instance
*(Slide 559)*

Run a docker container for a simple web server on an EC2 Linux instance
1. Install Docker engine:                  sudo yum install docker -y
2. Start Docker daemon                     sudo service docker start
3. Create sample index.html file locally
4. Create Dockerfile:                      FROM httpd COPY index.html /usr/local/apache2/htdocs/
5. Create image:                           sudo docker build -t mywebserver .
6. Run container:                          sudo docker run --name my-web-server -p 80:80 -d mywebserver
7. Access webserver over EC2 Public IP


## Containers Management and Orchestration
*(Slide 560)*


## Service
*(Slide 561)*

```text
      Monolith application architecture
    teams




                                                           Business
                                                            Logic

                                              UI Service


                                                           Database
```


## Service
*(Slide 562)*

```text
      Monolith application architecture
    teams


                                                                      Order
                                                                     Service

                                              Backend                                                    Database
                                              Service




                                                                                        Billing &
                                                                                        Payment
                                                   Queue                                                            CRM
               UI Service                          Service




                                                                               Search
                                                             Email
                                                                                                    Product
                                                                                                    Catalog
```


## Service
*(Slide 563)*

Monolith application architecture teams
Order Service Need container orchestrator for                              Database Backend
- Service    Container scheduling / placement
- Resource allocation
- Health check / Restart
- Creating and exposing services
- Network communication Billing &
- Monitoring                                Payment UI Service                         • QueueLogging                                                          CRM Service
- Authentication and authorization
- More..
Search Email Product Catalog


## What is orchestration?
*(Slide 564)*

*Control Plane   Data Plane*


## Opensource Container Orchestration Tools
*(Slide 565)*

```text
                                                Kubernetes


                                                Docker Swarm


                                               Apache Mesos


                                               OpenShift
                                                           +more..
                                                                                  Local workstation
                                                                                  Physical Servers
                                              Control Plane          Data Plane
                                                                                  Virtual Machines
```


## Opensource Container Orchestration Tools
*(Slide 566)*

```text
                                              Kubernetes




          Amazon EKS                Google GKE        Azure AKS


                      Cloud Implementation
                                                           +more..
                                                                                  Local workstation
                                                                                  Physical Servers
                                              Control Plane          Data Plane
                                                                                  Virtual Machines
```


## Container Orchestration services in AWS
*(Slide 567)*

```text
                                              User



                            Deploy container

                                                                Schedules and
                                                              runs the container
                                                                as per desired
                                                                 configuration




                                              ECS      EKS                         EC2    Fargate

                                              Control Plane                          Data Plane
```


## Amazon ECS
*(Slide 568)*


## Amazon ECS
*(Slide 569)*

Elastic Container Service
- Run and manage docker containers at scale without complexity                      ECS
- Supports 2 different compute options for running the containers
  - EC2 – Customer to manage (Supports Spot instances)
  - Fargate – AWS manages the infrastructure (Serverless) Serverless
- ECS integrates with many AWS services EC2              Fargate
  - ALB and NLB (as application backend service)
  - Amazon API Gateway (as API backend service)
  - Amazon EventBridge (for event-based execution)
  - Amazon CloudWatch (for metrics and logs)
  - Application Auto scaling and EC2 Autoscaling group
ECS is a good choice if you want a simpler, AWS-native container orchestration service with minimal operational overhead.


## How it works?
*(Slide 570)*

```text
                                    Build image and push
                                         to repository



          Developer                                             Amazon ECR
                                                                                            EC2 or Fargate



                                      Create ECS Cluster
                                                                                                             Docker
                                      Deploy Tasks / Services                             ECS agent
                                                                                                             runtime
            admin                                               Amazon ECS
                                                                                               ECS Tasks
                                                                                                                 launch




                                                                               Services
                                  Access application

                                                                                               ECS Tasks
              User
                                                                Elastic Load
                                                                 Balancing
```


## ECS Tasks and Services
*(Slide 571)*

{
- ECS Task:                                                        "family": "sample-task",
  - An ECS task is a single running instance of a          "networkMode": "awsvpc", task definition.                                       "requiresCompatibilities": ["FARGATE"],
  - Uses task definition to define container images,       "cpu": "256", CPU, memory, ports, environment variables and          "memory": "512", IAM roles to be used.                                  "taskRoleArn": "arn:aws:iam::112233445566:role/ S3ReadTaskRole",
- ECS Service:                                                     "containerDefinitions": [
  - An ECS service runs and maintains a specified            { number of tasks continuously.                              "name": "app", "image": "nginx:latest",
  - It automatically restarts failed tasks to maintain         "essential": true, the desired count.                                         "portMappings": [
  - Services integrate with Application or Network               { Load Balancers.                                                "containerPort": 80 } ] } ] }
Task definition


## ECS Features – Important to know
*(Slide 572)*

- IAM Roles for ECS
- Data Store for ECS
- ECS Autoscaling


## IAM Roles for ECS
*(Slide 573)*

ECS uses 2 types of IAM roles:                                              EC2 EC2 IAM Profile
- EC2 Instance Profile (EC2 Launch Type): Pull image
  - Used by the ECS agent
  - Send container logs to CloudWatch Logs ECS agent Docker
  - Pull Docker image from ECR Metrics &
  - Access secrets from Secrets Manager or parameters from                               Logs SSM parameter store CloudWatch
- ECS Task Roles:
  - Use different IAM roles for the individual ECS tasks                  TaskRole
  - Task Role is defined in the task definition as taskRoleArn
S3 TaskRole
ECS Tasks                      DynamoDB


## Persistent data store for ECS tasks
*(Slide 574)*

- For persistent shared storage in ECS, it’s a good choice to       EC2                         Fargate use Amazon EFS.
- Amazon EFS provides a managed linux file system that can be mounted by multiple tasks and is supported on both ECS on EC2 and ECS on Fargate.
- EFS volumes are mounted through the ECS task definition       mount     mount                     mount mount
- EFS supports multi-AZ access, making it suitable for highly available ECS services.
- Use cases
  - Applications allowing users to upload files
  - Shared configuration files or application assets
  - Stateful container workloads running on Fargate
Amazon EFS    File System


## ECS Service Auto Scaling
*(Slide 575)*

- ECS Service Auto scaling uses AWS Application Autoscaling service
- Amazon ECS publishes CloudWatch metrics with your service’s        EC2 or Fargate average CPU and memory usage
- It scales out by adding more tasks and scales-in by removing         ECS Service running tasks to achieve desired tasks count
- For Fargate launch type, underlying compute is managed by              Tasks AWS for scaling activities.
- For EC2 launch type, use EC2 Capacity provider which works with EC2 Autoscaling group (ASG) and scales the number of EC2 instances automatically. Amazon
- ECS Service Auto scaling supports following types of the scaling                    CloudWatch policies:
  - Target Tracking, Step scaling, Scheduled scaling and Predictive scaling


## ECS Service Auto Scaling
*(Slide 576)*

- Target Tracking: Automatically adjusts the number of ECS tasks to keep a metric (like CPU at 50%) steady, like how a thermostat maintains room temperature.
- Step scaling: Scales ECS tasks in predefined steps based on CloudWatch alarm thresholds, for example adding 1 tasks if CPU crosses 40% and adding 2 tasks if CPU crosses 60% likewise
- Scheduled scaling: Scales ECS tasks at specific times based on a schedule, such as increasing capacity every morning and reducing it at night.
- Predictive scaling: Uses historical metrics and ML forecasting to scale ECS tasks in advance of expected traffic spikes. Suitable for cyclic and recurring workloads.


## Let’s Talk Architecture - ECS
*(Slide 577)*


## ECS common architecture patterns
*(Slide 578)*

```text
         Hosting Application Backend services

                                                                 ECS Cluster




                                                             Service A         Tasks

                                                                                       DynamoDB
                                              Elastic Load
                                               Balancing
                                                             Service B         Tasks
```


## ECS common architecture patterns
*(Slide 579)*

```text
         Hosting API Backend services

                                                                       ECS Cluster




                                                                   Service A         Tasks

                                                                                             DynamoDB

                                              Amazon API Gateway

                                                                   Service B         Tasks
```


## ECS common architecture patterns
*(Slide 580)*

```text
         Event driven architecture for data processing

                                                                                                    ECS Cluster
                                                      Event notification
                                                                                         Run task


                                                                            Amazon
                                                                           EventBridge

                                 upload
                                                                               Download file
                                                                                                         Task
                                              Source Bucket



                                                                                  Output
                                               Target Bucket
```


## ECS common architecture patterns
*(Slide 581)*

```text
         Batch job processing and scaling based on SQS message depth

                                                                                                                    ECS Service Auto
                                                                                                                        Scaling

                                                                                         Amazon
                                              ApproximateNumberOfMessagesVisible       CloudWatch




                                                                                                    Poll messages
                                                         Send message
                                                          (Job request)
                                  Application

                                                                                   SQS Queue

                                                                                                                    Tasks
```


## Amazon EKS
*(Slide 582)*


## Amazon EKS
*(Slide 583)*

Elastic Kubernetes Service
- Kubernetes is most popular opensource and broadly used                    EKS container orchestration platform
- Amazon EKS runs vanilla Kubernetes which is upstream certified conformant version of Kubernetes                                             Serverless EC2            Fargate
- Amazon EKS supports 4 versions of Kubernetes
- Amazon EKS supports 2 different compute options for hosting the PODs (one or more containers)
  - EC2 – Managed node group by EKS
  - Fargate – AWS managed infrastructure
- EKS supports Spot instances as worker nodes in managed or self-managed node groups.
EKS is a good choice if you already have good experience managing and running Kubernetes cluster at scale in production environment


## Amazon EKS architecture
*(Slide 584)*

```text
                                    Build image and push
                                         to repository



           Developer                                       Amazon ECR




                                                                                             Docker
                                                                          kubelet
                                      Deploy PODs                                            runtime
           admin
                                                           Amazon EKS


                                                                                     launch




              User
                                                           Elastic Load
                                                            Balancing



                                                                                Containers
```


## AWS Fargate
*(Slide 585)*

- A Serverless compute for running containers on AWS ECS        EKS
- Runs docker containers on AWS managed infrastructure, No EC2 – no management !                                          Run containers (Tasks or pods)
- Just define container image, CPU, memory and Fargate will run the containers.                                                                        Serverless
- Pay-as-you-go pricing.                                                           Fargate
- Works with Amazon ECS tasks and Amazon EKS pods
- Fargate always uses awsvpc mode, where each task behaves like a first-class VPC resource with its own ENI and security groups.
- Monitoring using CloudWatch container insights
CloudWatch


## CloudWatch container insights
*(Slide 586)*

*https://aws.amazon.com/blogs/mt/introducing-container-insights-for-amazon-ecs/*


## Amazon ECR
*(Slide 587)*

- Amazon ECR is a fully managed container image registry used to store container images.
- Images are stored in private repositories by default, with support for public repositories.
- Integrates natively with Amazon ECS and EKS.
- ECR supports image versioning using tags and digests. Amazon ECR
- Authentication and access control are managed using IAM policies and roles.
- Images are encrypted at rest and in transit automatically. Registry
- ECR can perform vulnerability scanning of images using Amazon Inspector.
- Provides lifecycle policies to automatically delete old or unused images. Version 1
Version 2
Latest


## Running containers at on-premises location
*(Slide 588)*

- Amazon ECS Anywhere On-premises
- Amazon EKS Anywhere


## Amazon ECS Anywhere
*(Slide 589)*

On-premises        AWS Cloud Amazon ECS Anywhere Server
- Amazon ECS Anywhere provides support          ECS agent      Amazon ECS for registering an external instance such as an on-premises server or virtual machine (VM), to your Amazon ECS              SSM agent cluster.
- ECS control plane is in AWS with an                          AWS Systems agent running on your servers.                                 Manager
Containers


## Amazon EKS Anywhere
*(Slide 590)*

On-premises                       AWS Cloud Amazon EKS Anywhere Server
- Kubernetes clusters run on your own infrastructure and uses open-source Kubernetes, packaged and validated by AWS.                                                          Manage cluster
- AWS provides EKS Anywhere tooling to                                           Tools create, manage, and upgrade the Kubernetes clusters.                                                             ECR
- Allows clusters to connect to AWS services such as Amazon ECR for images and                                                CloudWatch CloudWatch for logs and metrics.                 Containers


# AWS Databases

*(Source: Slide 591)*


## Web
*(Slide 592)*

```text
                                                        Users                                    Browser
         myapp.com on AWS                                                                                                        CloudFront

                                                         Route53           myapp.com                                                                 Edge
                                                                                                                                                   Locations



                                                                        ELB
                                                                                    Auto
                                                                                   Scaling                                         Lambda

                                                Web                                                                                Video
                                               Server        EC2 E      EC2 E
                                                                   B           B                                                   Convert
                                                                   S           S                    Rekognition             S3                S3
                            SNS
                                                                                                              AI models                                    QuickSight
                                                                                                              enhancement

                                                App
                                                             EC2 EB     EC2 E
                                               Server                          B
                             SES                                   S           S

                                                                                                        Deploy custom
                                                                                                            model
                                                                                                                        Sagemaker
                                                                                                                                                             Data
                                                                                                                                                           Warehouse
                            SQS
                                               Database
                                              ElastiCache                              Graph
                                                 Cache                                Database
                                                                              Neptune            Kinesis                    S3              EMR
                                                                                                                                                               Redshift
                       CloudWatch             Relational
                                                  Multi-AZ                          NoSQL
                                              Database                             Database

                                                             RDS       DynamoDB                  Glue
```


## Evolution of applications and databases
*(Slide 593)*

*Mainframes                         Client-Server   3-Tier   Microservices*


## Today’s need
*(Slide 594)*

*What we used traditionally?      What is needed today?*

*A Relational Database         Purpose built databases*


## AWS Database services
*(Slide 595)*

```text
       Banking,              Leaderboards,     Shopping cart,        Content        Fraud detection,         IoT        Analytics,
       Finance,                real-time      Product catalog,    management,            social         applications,   Data Marts
      Bookings,                analytics,        Customer        personalization,     networking,      event tracking
      ERP, CRM                  caching          attributes          mobile         recommendatio
                                                                                       n engine




     Relational                In-memory      Key-value or        Document              Graph            Timeseries        Data
     Database                   Database        NoSQL             Database             Database           Database       Warehouse
                                               Database
```


## AWS Database services
*(Slide 596)*

```text
                                                              Generative AI
     Systems of record,
       Supply chain,
        health care,
          financial
                                              Documents              images          Audio



                                                0.1, 2.9, 0.1, 0.9, 1.0,…     Vector representation
                                                                              of data for similarity
                                                2.3, 9.2, 2.5, 9.4., 4.1,…    search e.g. Storing
                                                                              data embedding for
            Ledger
                                                7.1, 1.0, 0.3, 3.4, 1.1,…     Generative AI RAG
           Database




                                                                   Vector (search)
                                                                     Database
```


## AWS Database services
*(Slide 597)*

```text
       Banking,              Leaderboards,     Shopping cart,        Content        Fraud detection,         IoT        Analytics,
       Finance,                real-time      Product catalog,    management,            social         applications,   Data Marts
      Bookings,                analytics,        Customer        personalization,     networking,      event tracking
      ERP, CRM                  caching          attributes          mobile         recommendatio
                                                                                       n engine




     Relational                                 Key-value or                            Graph            Timeseries        Data
                               In-memory                           Document
     Database                                     NoSQL                                Database           Database       Warehouse
                                Database                           Database
                                                 Database




Amazon Amazon                                                                         Amazon             Amazon          Amazon
 RDS Aurora                    Amazon          Amazon              Amazon
                             ElastiCache      DynamoDB           DocumentDB           Neptune          Timestream        Redshift
```


## AWS Database services
*(Slide 598)*

```text
                                                              Generative AI

     Systems of record,                                                                                   Amazon
       Supply chain,                                                                                     OpenSearch
        health care,
          financial
                                              Documents              images          Audio
                                                                                                       Amazon Aurora

                                                0.1, 2.9, 0.1, 0.9, 1.0,…     Vector representation
                                                                              of data for similarity
                                                2.3, 9.2, 2.5, 9.4., 4.1,…    search e.g. Storing       Amazon RDS
                                                                              data embedding for
            Ledger                              7.1, 1.0, 0.3, 3.4, 1.1,…     Generative AI RAG
           Database

                                                                                                       Amazon Neptune


  Amazon Quantum                                                   Vector (search)
  Ledger Database                                                    Database                            Amazon
       (Amazon QLDB)                                                                                   DocumentDB
```


## AWS Shared Responsibility model
*(Slide 599)*

“Security of the Cloud” - AWS responsibility
- Protecting the infrastructure that runs all of the services offered in the AWS Cloud.
- Physical security of the data center facilities
- Hardware, software, networking for fully managed services like DynamoDB, S3
“Security in the Cloud” - Customer responsibility
- Depends on AWS service
- Typically protecting data, access, firewall configurations is customer’s responsibility


## Why managed database service?
*(Slide 600)*

```text
                                                 Schema design                    Schema design
                                               Query construction       You     Query construction
                                                 Schema design                  Query optimization
                                               Automatic fail-over
                                                                                Automatic fail-over
                                               Backup & recovery
                                                                                Backup & recovery
                                               Isolation & security
                You                                                             Isolation & security
                                              Industry compliance
                                                                               Industry compliance
                                              Push-button scaling
                                                                               Push-button scaling
                                              Automated patching
                                                                               Automated patching
                                              Advanced monitoring
                                                                               Advanced monitoring
                                              Routine maintenance
                                                                               Routine maintenance
                                              Built-in best practices         600
                                                                               Built-in best practices
                                                Self Managed                    Fully Managed
```


## AWS Shared Responsibility model for databases
*(Slide 601)*

- Public or Private network access to the
- Provision and manage the underlying EC2 instance       database hosting RDS database                               •   Security group inbound rules for database
- Automated backups (feature of RDS)                     instance (e.g. 3306 port to be opened for
- Underlying OS and DB patching                          MySQL DB)
- Database maintenance
- Enabling encryption for data at rest
- Database user creation and permissions
- Manual backups (on-demand)


## Amazon RDS
*(Slide 602)*


## Amazon RDS
*(Slide 603)*

- Amazon Relational Database Service
- A managed SQL database service where AWS handles provisioning, patching, upgrades, backup, recovery, repair, monitoring etc.
- RDS supports following database engines:
AWS native DB                   Open-source DBs
Commercial DB Amazon Aurora


## RDS Features
*(Slide 604)*

- RDS Multi-AZ deployment – Deploy database across multiple AZs
- RDS Read Replica – For scaling read queries
- RDS Storage Auto Scaling – Increase storage automatically
- RDS Custom – Get access to underlying host
- RDS Backup – Automated and manual backup
- RDS Security - Authentication / Encryption / Network


## Demo: Launch RDS DB and connect
*(Slide 605)*

```text
                                                               Internet
                                                                                                       High level steps
                                  Region                             AWS Mumbai Region

                                                                                         1   Launch an EC2 instance in the default VPC
                                        VPC                                                  and connect over SSH

                                                 Availability Zone

                                                                                         2
                                                                                             Create MySQL RDS database in the default
                                                                                             VPC (No public access). Security group to
                                         Web Server                                          allow traffic from EC2.


                                                                                         3   Install mysql client on EC2



                                                                                         4   Connect to RDS DB using database endpoint
```


## How to connect to Mysql RDS?
*(Slide 606)*

1. Install mqsql client in the ec2 instance
# install pip (Amazon Linux 2023 does not have one by default) $dnf install -y pip
# install dependencies $dnf install -y mariadb105-devel gcc python3-devel
# install mysqlclient $pip install mariadb105
2. Connect to Database and query the data
$mysql -h <database endpoint> -u admin –p $MySQL [(none)]> create database awswithchetan; $MySQL [(none)]> use awswithchetan;
3. Create table and add data. MySQL [corp]> create table students (emp_id int, name varchar(64), department varchar(32), location varchar(64)); MySQL [corp]> insert into students values (1001, ‘Chetan Agrawal’, ’IT’, ‘Pune'); Query OK, 1 row affected (0.003 sec)


## RDS Multi-AZ deployment
*(Slide 607)*

- Primary DB instance in one AZ and Standby DB instance(s) in another AZ                                                    Availability Zone                  Availability Zone
- Application reads from and writes to only Primary DB.
- Data is synchronously replicated from Primary DB to                                              Application Standby DB.
- In case of Primary DB failure or AZ failure, Standby DB                        Read/Write                           Read/Write is made Primary, and the DNS endpoint points to Standby. Synchronous
- Applications automatically gets redirected to standby                                             Replication for read/write queries. No change required at application end. Primary                                        Standby
Multi-AZ is a High Availability solution, not a scaling solution


## RDS Read replicas
*(Slide 608)*

- Create Read Replicas to scale the Read queries thereby Application relieving pressure on Primary DB                                                    Application Application
- Data is only written to the Primary DB and can be read from any of the read replicas                                  Read/ Writes
- Create up to 15 Read Replicas in the same or different                                Read         Read AWS region (for cross-region DTO charges apply) Replication
- Data is replicated asynchronously                                     (async)
- Promote Read replica to standalone DB in case of failure of Primary DB instance (Disaster Recovery).                Primary                     Read Replicas


## RDS Read replicas
*(Slide 609)*

- Create Read Replicas to scale the Read queries thereby Application relieving pressure on Primary DB                                                     Application Application
- Data is only written to the Primary DB and can be read from any of the read replicas                                   Read/ Writes
- Create up to 15 Read Replicas in the same or different                                 Read         Read AWS region (for cross-region DTO charges apply) Replication
- Data is replicated asynchronously                                      (async)
- Promote Read replica to standalone DB in case of failure of Primary DB instance (Disaster Recovery).                 Primary                     Read Replicas Use cases:
1. Business reporting and data warehousing where queries can run against a read replica, instead of production DB instance.
2. Implementing disaster recovery.
3. Read data with low latency from the local region even if Primary DB is in another region.


## RDS – Storage autoscaling
*(Slide 610)*

- Automatically increases storage capacity for an RDS DB instance when needed.
- Storage autoscaling happens only when all conditions are met:
  - Free storage falls below 10% of allocated storage.
  - Low-storage condition persists for at least 5 minutes.
  - At least 6 hours have passed since the last storage increase.
- You must configure a Maximum Storage Threshold, which defines the upper limit.
- Ideal for applications with unpredictable or spiky workloads.
- Supported for all RDS database engines (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server).
- Supported on gp2, gp3 and Provisioned IO volume types (not available for Magnetic volumes).


## RDS Custom
*(Slide 611)*

- Managed database service with OS/DB-level access - gives you SSH/RDP access to the underlying host, unlike standard RDS.
- Designed for customized, legacy, or packaged applications that require OS-level changes or special database configurations.
- Supports Oracle and SQL Server. SSH / RDP / SSM
- You can install custom patches, drivers, agents, and third-party software on the database host.
- RDS still automates many tasks (backups, monitoring), but you control more of the environment.
- Use when you need features not available in standard RDS, such as OS-level file changes, Custom database extensions, or Specific security tools
- Note: De-activate Automation Mode when you have to apply customizations. Also it’s recommended to take a DB snapshot before making the changes.


## RDS Backup
*(Slide 612)*

- RDS supports Automatics continuous backups and on-demand or scheduled backups in the form of snapshots.
- With continuous backups we can do Point in time restore (specific time with 1 sec granularity up to maximum 35 days retention)
- RDS manual snapshot are stored in S3 and can retain as long as you want.
- Snapshots can be copied across AWS accounts and regions to create a new database.
- RDS performs automated backups only within a daily backup window, which you can choose or otherwise chosen by AWS.
- Backups can introduce slight I/O impact, so choose a low-traffic      Automated Backup      Manual Backup time.                                                               (Snapshot + Trx Logs)    (Snapshot)


## RDS Security
*(Slide 613)*

Access/Authentication:
- Supports Username/password authentication for all databases. Recommended to use AWS Secrets Manager or SSM Parameter to store database credentials.
- Supports IAM authentication for MySQL & PostgreSQL DB engines. Encryption:                                                                              Authentication
- You can enable encryption for RDS DB at the creation time. Uses AWS KMS.
- Supports TLS/SSL encryption in transit for secure client connections.                         TLS
- Automatic backups and snapshots stored in S3 are encrypted if encryption is enabled for the database                                                               Private subnet
- If you need encryption later, you must take a snapshot -> copy it with encryption -> restore a new encrypted DB. Network:
- Full VPC network isolation using subnets, security groups, and NACLs.
- No public internet access unless explicitly configured.


## Amazon Aurora
*(Slide 614)*


## Amazon Aurora
*(Slide 615)*

- Aurora is PostgreSQL and MySQL compatible relational Primary                     Read Replicas database engines built by AWS. Aurora also supports DSQL (new).
- Aurora provide 5x the throughput of MySQL and 3x of PostgreSQL DB and 1/10th cost of commercial databases.
- Aurora DB cluster consist of Primary (writer) DB instance and Replicas (reader)                                                         Shared Storage volume
- Aurora maintains 6 copies of data across 3 Availability Zones with shared storage
- Aurora supports massive read scaling with up to 15 read replicas.
Availability      Availability     Availability Zone 1            Zone 2           Zone 3 Aurora’s shared distributed storage


## Amazon Aurora
*(Slide 616)*

- Aurora Global Database
- Aurora Serverless
- Aurora Cloning
- Aurora Machine Learning
- Aurora + RDS -> RDS Proxy
- Aurora + RDS -> RDS Reserved Instances


## Aurora Global Database
*(Slide 617)*

- Aurora global database spans across multiple regions
- Data is written to the primary DB in the primary region and is replicated to other regions (within 1 sec)
Use cases:
  - Bring data close to your customer’s P applications in different regions                         R
  - Promote read-replica to primary for faster recovery in the event of disaster R
R


## Aurora Cloning
*(Slide 618)*

- Aurora lets you create a clone of a database in minutes without copying all the data.
- Uses copy-on-write technology - only changed data blocks are duplicated.
- Extremely fast, cost-efficient, and ideal for creating Test, Development databases from production DB
- Faster than snapshot and restore
- You can create multiple clones (up to 15) from the same source cluster.
copy-on-write


## Aurora Serverless
*(Slide 619)*

- It’s hard to imagine having Relational Database as Serverless. Application
- No need to provision database for peak loads.
- With Aurora Serverless, you create a database, specify the desired database capacity range, and connect your applications.
- It automatically starts up, shuts down, and scales capacity up or down based on your application's needs.                                                                Compute +
- Pay on a per-second basis for the database capacity that you use.                                      Memory
- Supports full Aurora features, including global database, Multi-AZ deployments, and read replicas. Use cases:
  - All types of database workloads including development and test environments,   Shared Storage websites, applications having infrequent, intermittent or unpredictable workloads to business-critical applications requiring high scale


## Aurora Machine Learning
*(Slide 620)*

- Aurora Machine Learning allows your Applications to call machine-learning services directly from SQL queries, without writing application-level ML code.
- Aurora integrates with:
  - Amazon SageMaker AI – AI/ML platform
  - Amazon Comprehend – NLP, Sentiment Analysis
  - Amazon Bedrock – GenAI platform
- Aurora does NOT train or host models - inference happens in AWS ML services.
- Data stays in Aurora; only the necessary fields are sent securely to ML services. Example: Using SageMaker AI
- Useful for real-time intelligence: fraud detection, sentiment analysis, customer churn prediction,             AWS Blog: https://aws.amazon.com/blogs/database/part-1-adding-real-time- machine-learning-predictions-to-amazon-aurora/ recommendations, summarization, and classification.


## Comparison between RDS and Aurora
*(Slide 621)*

```text
                                                           Amazon RDS                                   Amazon Aurora

                        DB Engine               MySQL, PostgreSQL, MariaDB, SQL                   MySQL, PostgreSQL, DSQL
                                                      Server, Oracle, DB2
                       Architecture                 Compute + Storage coupled.                 Decoupled compute and Storage

                    Read Scalability             15 Read replicas, 5 for Oracle DB                      15 Read replicas

                          Storage             Up to 64TB (option to auto scale storage)   Up to 128TB (auto scaled in 10GB increment)

                             HA                   Multi-AZ with 60-120 sec failover        Automatic failover using Replica (< 30 sec)

                            Cost                    Low as compared to Aurora                     Higher as compared to RDS

                      Performance                   Low as compared to Aurora                     Higher as compared to RDS

                 Important Features              Automated backup, Multi-AZ, Read           + Global database, Serverless, Cloning,
                                                            Replicas                              Aurora Machine Learning
```


## RDS Proxy
*(Slide 622)*

Why database proxy?
- Serverless and modern apps often create many short-lived DB connections, which can exhaust DB memory/CPU.
- Proxy allows sharing database connections, reducing overhead on the DB.


## RDS Proxy
*(Slide 623)*


## RDS Proxy
*(Slide 624)*

VPC
- Fully managed database proxy for RDS and Aurora.                                Lambda           ECS Tasks
- Sits between your app and DB to manage and pool connections efficiently.
- Reduces failover time by up to 66% for RDS and Aurora databases.
- RDS Proxy can be enabled for most applications with no code changes Signed IAM authentication token
Security
- Enhances security by storing credentials in AWS Secrets Manager. Apps can optionally use IAM token authentication for proxy
- RDS Proxy is not publicly accessible; it always lives inside a VP                   Secrets Manager
Use case: Ideal for serverless apps with unpredictable or high-concurrency workloads.


## RDS Reserved Instances
*(Slide 625)*

- Reserved Instances are a great option for the steady state use case and can save up to 69% cost over On-Demand for RDS and Aurora
- RDS Reserved Instances provide three payment options: All Upfront, Partial Upfront, and No Upfront.
- Reserved Instances offers instance size flexibility for Amazon Aurora, MySQL, MariaDB, PostgreSQL, and DB2 database engines as well as the “Bring your own         Reserved Instances license” (BYOL) edition of the Oracle database engine.
- All Upfront and Partial Upfront Reserved Instances can be purchased for one or            = three-year terms and No Upfront Reserved Instances are only available for one year term.


## Amazon ElastiCache
*(Slide 626)*


## Caching
*(Slide 627)*

Browser
- Caching is not limited to a single component of an application CDN
- Each layer serves a different purpose
- Modern applications (e.g., eCommerce) commonly use a multi-layer caching strategy    Amazon CloudFront for optimal speed, scale, and cost.
- Caching can be done at                                                  Application
  - Client-side caching
  - Content Delivery Network
  - Application Layer
  - Distributed Cache
  - Database Cache (buffer) In-memory   Database Amazon ElastiCache


## Amazon ElastiCache
*(Slide 628)*

- Database Caches are in-memory databases with high performance                             Application and extremely low latency to read/write data
- Helps reduce load off database I/O for read intensive workloads.
Write
- Different caching strategies: Lazy load, Write-through, TTL
Read 1                  3
Read
- Serverless, Multi-AZ, Cross region replication and Global                                                        2 Datastore                                                                                                 If cache miss
- Amazon ElastiCache supports following DB cache engines:                        ElastiCache for Redis
  - Redis OSS - Rich data structures, pub/sub, geospatial, streams etc.
  - Memcached - Simple, multi-threaded, horizontal scaling
  - Valkey - Open-source Redis-compatible engine (community-driven), designed to avoid Redis licensing restrictions.                                              Amazon RDS Use cases: Session stores, Gaming leader boards, Online shopping cart


## Comparison between Redis and Memcached
*(Slide 629)*

```text
                                                               Redis                                    Memcached

                     Authentication           Supports AUTH (data-plane). IAM for API         No built-in AUTH; supports SASL
                                                             actions                             authentication (advanced).
                        Encryption                TLS/SSL in-transit + KMS at rest         No encryption at rest; limited in-transit
                                                            encryption.                                  support.
                    High Availability           Supports Multi-AZ with auto-failover.               No Multi-AZ failover.

                       Replication               Supports replication groups + Read                     No replication
                                                              replicas.
                Backup and Restore                Supports automatic backups and               No persistence, no backups.
                                                            snapshots.
                         Features             Supports Global Datastore (global reads +                Not supported
                                                                DR)
                        Use cases               Session store, leaderboards, caching      Simple key-value caching, multi-threaded
                                                     complex objects, pub/sub                 very high throughput workloads
                  Network Features                 Runs inside a VPC; Security Groups, NACL and Subnet Route table rules apply
```


## SQL vs NoSQL
*(Slide 630)*

- Need to run ad-hoc queries?
- 90-95% of queries are pre-defined?
- Predictable traffic?
- Need consistent performance at any scale?
- Schema is mostly fixed?
- Need flexible schema?
Example: Flight booking system, Banking     Example: eCommerce product catalog, multiplayer games
SQL Database NoSQL Database
Oracle, MSSQL, Amazon Aurora,       MongoDB, Cassandra, Amazon DynamoDB, PostgreSQL, MySQL               Apache CouchDB, Azure CosmosDB


## Amazon DynamoDB
*(Slide 631)*

- Amazon DynamoDB is a fully managed, serverless NoSQL (key-value) database service
- Consistent, single-digit millisecond read and write performance at any scale – millions of requests/seconds, trillions of row, 100s of TB of storage
- Serverless: No provisioning or capacity management, Encryption for data at rest, 99.999% availability
- Support different data types i.e. Scalar (Number, String etc.), Document (JSON) and Set types
Partition Key     Sort Key        Attributes DynamoDB has Table as basic entity to store data                       item Primary Key
- Table contains items (like rows)
- Items contains Partition Key, Sort Key (optional), and           item Primary Key attributes (key-value pairs).
- Partition Key + Sort Key = Primary Key                           item          Primary Key
DynamoDB Table


## Amazon DynamoDB – Query and Scan
*(Slide 632)*

Query - fetches specific items using Partition Key                     PartitionKey   SortKey
- Retrieves items by Partition Key (and optional Sort Key conditions).
- Can use conditions like <, >, <=, >= etc. with Sort Key
- Fast and efficient — looks only in matching partitions.
- Can use filters, but filtering happens after reading the data.
aws dynamodb query \ --table-name Orders \ --key-condition-expression "UserId = :userid" \ --expression-attribute-values '{":userid":{"N":"103"}}'
Orders Table


## Amazon DynamoDB – Query and Scan
*(Slide 633)*

Scan – fetches every item of the table                             PartitionKey   SortKey
- Reads every item in the table or index.
- Slow and expensive for large tables.
- Can use filters to narrow down the results but still scans all items first.
aws dynamodb scan \ --table-name Orders \ --filter-expression "Product = :name" \ --expression-attribute-values '{":name":{"S":"Widget"}}'
Orders Table


## DynamoDB - Global Secondary Index
*(Slide 634)*

Most recent order?
- A GSI is an index with its own Partition Key and optional PartitionKey   SortKey Sort Key, different from the base table’s keys.
- It lets you query the same table in a completely different way (new access pattern).
- They maintain a copy of selected attributes from the base table (based on Projection).
- GSIs have their own read/write capacity (in provisioned mode).
- Updates to the base table are automatically propagated to the GSI.
- Useful when your application needs multiple query patterns that the primary key alone cannot support.
Orders Table


## Example – Global Secondary Index
*(Slide 635)*

```text
                                              What is the top score ever recorded for the game Meteor Blasters?
PartitionKey SortKey




                                                GameScores Table                                      GameTopScores Index
```


## Amazon DynamoDB features
*(Slide 636)*

- DynamoDB Table class
- DynamoDB Read/Write Capacity (RCU/WCU)
- DynamoDB Global Tables
- DynamoDB Accelerator (DAX)
- DynamoDB Streams
- DynamoDB TTL
- DynamoDB Backup
- DynamoDB export / import


## DynamoDB Table Class
*(Slide 637)*

DynamoDB supports Standard Table class and Infrequent Access (IA) Table class
Standard (default)
- Best for frequently accessed data.
- Higher storage cost, lower read/write cost.
- Ideal for hot or regularly used items.
Standard-IA (Infrequent Access)
- Lower storage cost, higher read/write cost.
- Designed for rarely accessed or cold data.
- Useful for archival, history tables, old session data, etc.
- We can change the table class from Standard to IA


## DynamoDB Read and Write capacity
*(Slide 638)*

- We can define the DynamoDB Read and Write capacity as per the application need.
- There are two modes to set the capacity:
Provisioned Capacity Mode (Default)                    On-Demand Capacity Mode
- You pre-define RCU/WCU for the table.
- No need to set RCU/WCU - DynamoDB scales automatically.
  - Best when you have predictable or steady traffic.                                 24 hrs    • Ideal for unpredictable or spiky workloads.
- Cheaper than on-demand if your workload
- You pay per request, which may be costlier is consistent.                                       for steady high throughput.
- Risk of throttling if traffic suddenly spikes
- Virtually no throttling unless you hit above provisioned limits.                            account-level limits.


## DynamoDB Global table
*(Slide 639)*

- Multi-region, multi-active, serverless tables across regions                                      Users across the world
- 99.999% availability
- Can read/write to any replica Global Application
- DynamoDB replicates data across all replicas                                                              Replica (London) Use cases:
- Global application requiring low latency access for users
- Can handle region level failure (DR)         Replica (N. Virginia)                             Replica (Mumbai)


## DynamoDB Accelerator - DAX
*(Slide 640)*

- Fully managed highly available in-memory cache for DynamoDB            Your applications
- 10x performance improvement with single digit millisecond to microsecond level latency
- API-compatible with DynamoDB. Only client & endpoint needs to change to use with an existing application.
- DAX provides access to eventually consistent data from DynamoDB tables DAX
Use cases:
- Real-time bidding, social gaming, and trading applications Anti-pattern:
- Strongly consistent read, Write intensive, not having repeated reads      DynamoDB


## DynamoDB Streams
*(Slide 641)*

- Captures the item level modifications in time-ordered sequence
- Stores the changes in the logs exactly once and strictly ordered for 24 hours DynamoDB
- You can enable/disable a stream on a new or existing table                                 Table
- DynamoDB Streams operates asynchronously, so there is no performance impact on a table if you enable a stream. DynamoDB Stream Use cases:
- Real-time monitoring e.g. connected vehicles, sensor data, Notifying everyone on an activity e.g. friend creates a post on social media
- Backup/Change data capture of DynamoDB table SNS   Lambda     Kinesis


## DynamoDB TTL (Time To Live)
*(Slide 642)*

- TTL lets you automatically delete expired items based on a timestamp attribute. Current time >= 1733151000
- You specify a numeric attribute (e.g. expiresAt) containing a Unix epoch time (in seconds).                       userId        sessionId          expiresAt
- DynamoDB checks items in the background and                   11111            a0b1c2d3           173315100 removes expired ones asynchronously                                                               0
- No extra cost for TTL deletion; only standard                 22222            b0b1c2d4           173326107 read/write charges apply when you insert/update                                                   6 items.                                                                       User session table 33333            c0b1c2d5           173585923
- Helps reduce storage cost and keeps tables clean. 7
- Use cases:
  - User sessions/tokens - Automatically remove expired login sessions or JWT tokens.
  - Rate-limiting counters - Keep count of API hits per minute and auto-reset them via TTL.
  - OTP / verification codes - Remove expired OTP codes, email verification tokens, etc.


## DynamoDB Backup
*(Slide 643)*

DynamoDB supports Point In Time Restore and On-demand manual backups Point In Time Restore (PITR)
- PITR continuously backs up the DynamoDB table
- PITR allows restore the DynamoDB table to any second in the last 35 days.
- The recovery creates a new table
On-demand Backup
- On-demand backup creates a full manual snapshot that you can keep indefinitely.
- Can use AWS Backup service to create the backups including cross-region copy
- Restoring a backup always creates a new table.


## DynamoDB Export / Import
*(Slide 644)*

DynamoDB supports exporting data to S3 and importing data from S3
Export to S3
- Uses PITR snapshots, so you can export data from a specific point in time.           export
- Export does not consume DynamoDB Read/Write capacity
- Useful for analytics, data pipelines, and long-term data storage. Import from S3 analytics
- Allows you to import data directly from Amazon S3 into a new DynamoDB table.
- Supports CSV, DynamoDB JSON, and ION formats.
- No write capacity consumed during import (doesn't use WCUs).
- Import creates a new table — it cannot overwrite an existing one.                Athena
- Useful for bulk loading, migrations, and rebuilding tables from exported data.


## Amazon DynamoDB - Summary
*(Slide 645)*

- Amazon DynamoDB is a fully managed, serverless cloud-native NoSQL database service that provides millisecond latency at any scale
- Has Partition key, Sort key (optional) and attributes. Partition key + Sort key = Primary Key
- Query and Scan operations – Query fetches specific item, Scan fetches all items in the table. Optionally use GSI.
- Supports Standard Table class and Infrequent Access (IA) Table class
- Supports Provisioned Capacity mode (default) and On-demand capacity mode (RCU/WCU)
- Supports Global tables (Active-Active) for global applications.
- Support DynamoDB accelerator (DAX) providing microsecond latency
- Supports Streams to act on table level modifications. Integrate with other AWS services like Lambda, SQS, SNS. Kinesis for stream processing.
- Supports Time-To-Live (TTL) to automatically expire and delete the items from the table
- Supports PITR and On-demand backups. PITR allows restoring a new table (up to 35 days)
- Supports Exporting data to S3 and importing data from S3


## What is document?
*(Slide 646)*

{ "ProductID": "12345", "Category": "Mobile Phones", "Brand": "ExampleBrand", "Model": "ExampleModel X1",
- A JSON-like schema with nested key-value          "Specifications": { structure                                             "ScreenSize": "6.5 inches", "Resolution": "1080x2400 pixels", "Processor": "Octa-core 2.3 GHz", "RAM": "8 GB", "Storage": "128 GB", "Battery": "4000 mAh", "OS": "ExampleOS 12", Use cases:                                                  "Camera": { "Rear": "48 MP + 8 MP + 5 MP", "Front": "16 MP"
- User profile information (like on LinkedIn)           }, "Dimensions": {
- Product details, reviews (eCommerce)                      "Height": "160 mm", "Width": "74 mm", "Depth": "8 mm", "Weight": "180 g" }, "Connectivity": ["Wi-Fi", "Bluetooth 5.0", "NFC", "4G LTE"], "Ports": ["USB Type-C"] }, "Price": 699.99 }


## Amazon DocumentDB
*(Slide 647)*

- Amazon DocumentDB (with MongoDB compatibility) is a fast, reliable, and fully managed database.
- Used to store, query, and index JSON-like documents
- DocumentDB storage automatically grows in increments of 10GB, up to 64 TB.
- Automatically scales to workloads with millions of requests per seconds.
Compatible Use cases:
- User profile                                                        Amazon DocumentDB
- Content management like blogs, video metadata
- E-commerce product catalog (simiar to DynamoDB)


## Amazon Neptune
*(Slide 648)*

- Graph databases store data as a network of entities and             Movies relationships.
- Graph consists of Node and Edges where Node represents the                             D object (e.g. person, game) and edge represents relationship between the nodes (e.g. is friend of, plays).                                A                     C
- Amazon Neptune is Serverless Graph database (with all the E features such as HA, replication etc.)                                            B
- Can analyse graph datasets with tens of billions of relationships within seconds using built-in algorithms Books
- Can perform similarity searches on vectors stored along with your graph for gen AI apps.
Use cases: Social networking, Fraud detection, Recommendation,                              Amazon engines, Route optimization, Knowledge graphs (e.g. Wikipedia)                              Neptune


## Amazon Timestream, Amazon QLDB
*(Slide 649)*

Amazon Timestream                        Amazon QLDB                                        Holds current value and historical state of data
- Quantum Ledger Database
- Time series Database                                                     JOURNAL
- Immutable chain or records
- Up to 1000 times faster and 1/10th
- Cryptographically verifiable of cost of Relational databases log of data changes            Appends only crypto verified entries
- InfluxDB compatible
Use cases:                                Use cases:
- Live analytics
- Financial records
- Real time IoT data ingestion and
- Supply chain system analytics                          •   Claim history
- Real time Web traffic, Operational
- Trace and Track`ing systems metrics                                e.g. spare parts/inventory movement


## Exam Scenarios
*(Slide 650)*


## AWS Databases - Exam Scenarios
*(Slide 651)*

1. Scenario: A global e-commerce company wants an active-active database across multiple regions so customers can place orders from anywhere with low latency.
Key-words: Active-Active, multiple regions, place orders, from anywhere, low latency
Answer: DynamoDB Global Tables
Why not Amazon Aurora Global Database -> Place orders is a write operation and Aurora supports Read-replicas across the regions


## AWS Databases - Exam Scenarios
*(Slide 652)*

2. Scenario: A ride-sharing app needs to cache frequently accessed data like driver locations to reduce database load and achieve microsecond latency.
Key-words: Cache
Answer: ElastiCache or DynamoDB Accelerator (DAX)
It could be either of ElastiCache and DAX and if answer just contains one of them then it’s an easy pick. However, if both the options are there, then look for additional information e.g. source database is DynamoDB then it will be DAX or if the source database is RDS/Aurora or it mentions about advance features then it will be ElastiCache


## AWS Databases - Exam Scenarios
*(Slide 653)*

3. Scenario: A streaming platform wants to store large volumes of user activity logs but only needs them for occasional analytics queries.
Key-words: occasional, analytics queries
Answer: DynamoDB IA Table class or DynamoDB export to S3
Depending on additional context, if its about saving the storage cost then answer will be DynamoDB Infrequent Access Table class or if its about analyzing the data using Athena or other tools then answer will be exporting the data to S3.


## AWS Databases - Exam Scenarios
*(Slide 654)*

4. Scenario: A ride-sharing platform wants to quickly determine the shortest path between drivers and riders based on a complex graph of roads and connections.
Key-words: shortest path, graph, connections
Answer: Amazon Neptune


## AWS Databases - Exam Scenarios
*(Slide 655)*

5. Scenario: A team needs to quickly create a full copy of their production database to run performance tests and simulate heavy workloads without affecting the live system. The copy should be created within minutes.
Key-words: full copy of production, within minutes
Answer: Aurora Cloning
You may think of Read replicas but the scenario doesn’t mention about only reading the data. Further, Aurora’s decoupled and shared storage makes the cloning really fast (copy-on-write) and hence the time to create database clone is faster than other possible options.


## DynamoDB DAX vs ElastiCache
*(Slide 656)*

DynamoDB DAX                             ElastiCache
- Purpose-built cache only for DynamoDB and
- General-purpose, in-memory cache with fully API-compatible with DynamoDB.                advanced features like pub/sub, sorted sets, streams, Lua scripts, etc.
- Can cache any type of data - SQL queries, API
- Best for repeated read queries on DynamoDB         responses, session data, ML results, etc. data (hot keys, strongly read-heavy apps).
- Requires your application to implement cache
- No need to manage invalidation — DAX auto-         logic + invalidation. syncs with DynamoDB.
- When to use?
- When to use?                                          ✓ You need a cache for multiple systems ✓ You want to speed up DynamoDB reads        ✓ You want advanced capabilities (counters, without changing code.                       leaderboards, TTL per key, real-time messaging). ✓ You want to cache complex data models or large aggregated results.


# Bigdata and Analytics

*(Source: Slide 657)*

### Big data and Analytics


## Web
*(Slide 658)*

```text
                                                        Users                                      Browser
         myapp.com on AWS                                                                                                            CloudFront

                                                         Route53            myapp.com                                                                    Edge
                                                                                                                                                       Locations



                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                            Lambda

                                                Web                                                                                    Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                                      Convert
                                                                    S           S                      Rekognition              S3                S3
                            SNS                                                                                                                            BI /QuickSight
                                                                                                                  AI models
                                                                                                                  enhancement                          Visualization
                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                            Deploy custom
                                                                                                                model
                                                                                                                            Sagemaker
                                                                                                                                        Big data         Data
                                                                                                                                       processing      Warehouse
                                                                                              ClickStream          Data Lake
                            SQS
                                              ElastiCache

                                                                               Neptune             Kinesis                      S3              EMR
                                                                                                                                                                   Redshift
                       CloudWatch                  Multi-AZ                                             ETL


                                                              RDS       DynamoDB                    Glue
```


## Before we move..
*(Slide 659)*

- Bigdata
- OLTP & OLAP
- Data warehouse
- Data Lake
- ETL
- Data formats – CSV / Parquet / Iceberg


## What is Big Data?
*(Slide 660)*

Big Data refers to extremely large datasets that cannot be easily managed, processed, or analysed using traditional data processing techniques.
Big data is characterized by Three V’s: Value                Volume
- Volume: The amount of data generated every second from various sources like social media, sensors, transactions, etc. Visualization
- Velocity: The speed at which new data is generated and needs     Veracity to be processed. This includes real-time data feeds and                         Big Data streaming data. Variability
- Variety: The different types of data, such as structured data (databases), semi-structured data (XML, JSON), and unstructured data (text, images, videos). Velocity Variety


## Big data frameworks..
*(Slide 661)*

Big data frameworks are instruments that simplify the processing of big data.
Popular Big Data frameworks
- Hadoop: Open-source batch-processing framework for the distributed storage and processing of big data sets. Hadoop operates by splitting files into large blocks of data and then distributing those datasets across the nodes in a cluster. Uses HDFS, MapReduce, YARN.
- Apache Hive: For data summarization, query, and analysis. Queries data from HDFS.
- Apache Hbase: A distributed big data store that supports structured data storage for large tables
- Apache Spark: In-memory real-time processing, batch processing
- Presto: Distributed SQL query engine optimized for running interactive analytic queries against data sources of all sizes ranging from gigabytes to petabytes.
- Apache Pig, Apache Sqoop, Apache Flume, Apache Mahout and more..


## OLTP Databases
*(Slide 662)*

```text
         OLTP vs OLAP

                                                      ERP      CRM

                                              Read                       Transform &
                                                                       de-normalise data
                                   App 1      Write
                                                            Database
                                              Read
                                   App 2      Write

                                                            Database                                   Business
                                                                                             Data
                                               Read                                        Warehouse   Analysts

                                   App 3
                                               Write

                                                            Database
```


## OLTP Databases
*(Slide 663)*

```text
                                          OLTP                                               OLAP
                 Online transaction Processing                                       Online Analytical Processing


                                                 Read                  Transform &
                                                                     de-normalise data
                                   App 1         Write
                                                          Database
                                                 Read
                                   App 2         Write

                                                          Database                                             Business
                                                                                           Data
                                                  Read                                   Warehouse             Analysts

                                   App 3
                                                  Write

                                                          Database
```


## OLTP Databases
*(Slide 664)*

OLTP                                                OLAP Online transaction Processing                                       Online Analytical Processing
Read                Transform & de-normalise data App 1 OLTP Write                                      OLAP Database
- To process transactions
- To analyse aggregated data Read
- Optimized for continuous Writes & small
- Optimized for Batch Writes & high reads App 2             Write                              volume read
- Data is highly normalized
- Data is denormalized Database                                                     Business
- Distributed databases
- CentralizedData database
- ACID properties          Read
- Data volume  – in order of TBs/ PBsAnalysts Warehouse
- Data volume – in order of GBs
- Example: Data warehouse, analyse App 3
- Example: Order management, Write Ticket                      market trends, Predict customer booking, ATM, Banking transactions Database                   behaviour, sales reports


## OLTP Databases
*(Slide 665)*

```text
         Data Warehouse
       A data warehouse is a system that stores and organizes data from multiple sources for reporting and
       analysis

                                              Read                  Transform &
                                                                  de-normalise data
                                   App 1      Write
                                                       Database
                                              Read
                                   App 2                               ETL
                                              Write

                                                       Database                                     Business
                                                                                        Data
                                               Read                                   Warehouse     Analysts

                                   App 3
                                               Write

                                                       Database
```


## OLTP Databases
*(Slide 666)*

```text
         Data Lake
      A data lake is a centralized repository for storing, processing, and securing large amounts of data




                                   App 1                                            Database




                                   App 2      Store    01000111100101010100101010
                                                        1010101010100101010101010
                                                          1010101010101010101010
                                                             10100101010101010
                                                                                         Data          Business
                                                                                       Warehouse       Analysts
                                                         Data Lake
                                   App 3


                                                                                    Machine Learning
```


## Common Data Storage Formats in Data Lakes
*(Slide 667)*

Row-based format              Columnar-based format           Table-based format
- Stores data by rows
- Stores data by columns
- Stores data by columns (generally in parquet / ORC format)
- Simple and Human-readable
- High performance for analytics                    • High performance for transactional
- Poor performance for analytics                                      data lakes
- Supports compression
- No support for schema
- Supports ACID evolution, versioning and          • Designed for Data lakes transactions                                                      • Supports schema evolution
- Optimized for engines like Spark, Athena, Redshift      • Supports Updates/deletes spectrum
- Supports Time travel
Ex: CSV, JSON, TSV              Ex: Apache Parquet            Ex: Apache Iceberg


## Row based storage
*(Slide 668)*

*Column based storage*


## AWS Data Analytics services
*(Slide 669)*


## OLTP Databases                      OLAP Databases
*(Slide 670)*

```text
                                                                       AWS Glue
                                                      ERP     CRM
                                                                                    Transform &
                                              Read                                De-normalise data

                                   App 1      Write
                                                                        EMR


                                                              RDS
                                              Read
                                   App 2                                ETL
                                              Write


                                                            DynamoDB                  Amazon           Amazon
                                                                                      Redshift        QuickSight
                                               Read

                                   App 3
                                               Write


                                                            Amazon
                                                            Aurora
```


## Data Lake                 OLAP Databases
*(Slide 671)*

```text
                                                                  AWS Glue
  Kinesis           MSK                    MSF      ERP    CRM
                   (Kafka)                (Flink)                              Transform &
                                                                             De-normalise data

                                                                   EMR
   IoT
 Sensors


CV                                                                 ETL

                                                      Amazon S3                  Amazon           Amazon
                                                                                 Redshift        QuickSight
     Click
     data                                                                            SQL

                                                                  Amazon
          Real-time data ingestion                                Athena
```


## AWS Glue
*(Slide 672)*


## Web
*(Slide 673)*

```text
                                                        Users                                 Browser
         myapp.com on AWS                                                                                                     CloudFront

                                                         Route53            myapp.com                                                             Edge
                                                                                                                                                Locations



                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                     Lambda

                                                Web                                                                             Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                               Convert
                                                                    S           S                Rekognition             S3                S3
                            SNS
                                                                                                           AI models                                    QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker


                            SQS
                                              ElastiCache

                                                                               Neptune        Kinesis                    S3              EMR
                                                                                                                                                            Redshift
                       CloudWatch                  Multi-AZ                                        ETL


                                                              RDS       DynamoDB              Glue
```


## AWS Glue
*(Slide 674)*

- Serverless Data integration and ETL (Extract, Transform, and Load) service.
Data Source                                                             Data Target Transform
Extract                        Load
Script
Extract/Prepare                   Transform                           Load


## AWS Glue
*(Slide 675)*

- Serverless Data integration and ETL (Extract, Transform, and Load) service.
- Automatic Schema discovery and Data cataloging with Glue Crawler.
- Supports Data transformation (using Apache Spark / Python shell / Ray engines)
Data Catalog
70+ Data sources
Transform
crawl Code AWS Glue
DynamoDB


## AWS Glue
*(Slide 676)*

- Serverless Data integration and ETL (Extract, Transform, and Load) service.
- Automatic Schema discovery and Data cataloging with Glue Crawler.
- Supports Data transformation (using Apache Spark / Python shell / Ray engines)
Data Catalog Glue Job
70+ Data sources Transform            S3 Extract                           Load
Code             Amazon AWS Glue                      Redshift
DynamoDB


## AWS Glue Features – Good to know
*(Slide 677)*

- Glue Data Catalog and Crawlers
- Glue Jobs and Bookmarks
- Glue Studio
- Glue Streaming ETL
- Glue Data encryption


## Glue Data Catalog and crawlers
*(Slide 678)*

- Glue Data Catalog is a central metadata repository which acts as an index to the location and schema of the data sources
- Glue Data Catalog can be used by other AWS analytics services:                    Glue Data Catalog
  - Amazon Athena
  - AWS Lake Formation
  - Amazon EMR IAM Role
  - More.. Crawler
- Data Catalog is automatically created and updated using Glue Crawlers
- You can run crawler on demand or on a schedule
- If the data source is an AWS service (for example, S3), the crawler IAM role must have permissions to read the S3 bucket or prefix.                         Application


## Glue Jobs
*(Slide 679)*

- Define data source and target
- Define ETL jobs with transformation scripts to move and process data.
- Run jobs on-demand or based on triggers.
- Monitor job performance using dashboards.


## Glue Job bookmarks
*(Slide 680)*

- Glue Job Bookmark remembers what data has already been processed by a Glue ETL job, so the next run processes only new or changed data. Source
- It saves the job run details like processed file paths, partition values,             new data timestamps and job run state
- You can enable, disable or pause Job bookmark feature
- Without bookmarks:
  - Every job run reads all data again Job bookmark
  - Higher cost, time, and duplicate processing
- With bookmarks:
  - Glue processes only new files/records
  - Faster and cheaper ETL
Target


## AWS Glue Studio (Visual ETL)
*(Slide 681)*

- AWS Glue Studio provides a visual interface for creating, running, and monitoring ETL jobs in AWS Glue.


## Exercise: csv to parquet file conversion with Glue
*(Slide 682)*

1. Create source and target S3 bucket and load csv data in source bucket
2. Create an IAM role for Glue to allow S3 read/write and Glue job related permissions                                                             Source                       csv
1. Include AWSGlueServiceRole IAM policy
2. Create and add policy to include permissions for S3 read for source                     read bucket and S3 write for target bucket. (Sample policy provided in the lecture resources. Replace your bucket names.)              + GlueServiceRole
3. Create Glue job using Glue studio UI Glue Job
4. Configure Source, Transformation and Target steps
1. Source => S3 bucket / prefix (Data Format => CSV)
2. Transformation => Change Schema (Choose columns to include or                            write drop)
3. Target => S3 bucket (Data Format => Parquet)
5. Save and run the job                                                               Target       parquet


## Sample IAM policy for S3 permissions
*(Slide 683)*

```text
        {
            "Version": "2012-10-17",
            "Statement": [
              {
                "Sid": "ReadFromSourceBucket",
                "Effect": "Allow",
                "Action": [
                  "s3:GetObject",
                  "s3:ListBucket"
                ],
                "Resource": [
                  "arn:aws:s3:::chetan-glue-demo-source-bucket",
                  "arn:aws:s3:::chetan-glue-demo-source-bucket/*"   Replace bucket names
                ]
              },
              {
                "Sid": "WriteToTargetBucket",
                "Effect": "Allow",
                "Action": [
                  "s3:PutObject",
                  "s3:ListBucket"
                ],
                "Resource": [
                  "arn:aws:s3:::chetan-glue-demo-target-bucket",
                  "arn:aws:s3:::chetan-glue-demo-target-bucket/*"   Replace bucket names
                ]
              }
            ]
        }
```


## Apache parquet
*(Slide 684)*

- Apache Parquet is a columnar storage file format designed for analytics.
- Stores data column-wise (not row-wise like CSV)
- Much smaller file size due to compression
- Faster queries with Athena / Redshift Spectrum
- Ideal format for data lakes on S3


## *Image generated by Chatgpt
*(Slide 685)*


## Glue streaming ETL
*(Slide 686)*

- Allows ETL for real-time streaming data
- Supports Kinesis data stream and Kafka source and S3/JDBC as target
- Use Streaming ETL in AWS Glue to process event data like IoT streams, clickstreams, and network logs.
Streaming engine             Cleanse and Transform
IoT Sensors S3
Kinesis   Amazon MSK Click Data Data Streams
Network Logs                                                                                    JDBC


## Glue Data Encryption
*(Slide 687)*

Client Data in Transit:
- Supports encryption for data in transit using TLS endpoints and TLS           Source connections for source and the targets                                         data
Data at Rest:                                                                            TLS
- Supports AWS KMS to enable encryption for Glue data catalog and Cloudwatch Logs generated by Glue crawler
- For S3 target, Use Security Configuration and attach it to Glue ETL jobs to enabled SSE-S3 or SSE-KMS encryption KMS TLS
SSE-S3 Target SSE-KMS


## Amazon EMR
*(Slide 688)*

*Elastic Map Reduce*


## Amazon EMR
*(Slide 689)*

- Amazon EMR (Elastic MapReduce) is used for general big data RDS        S3     DynamoDB processing and analysis.
- Supports Hadoop Map Reduce and Apache Spark, Hive, Presto and              Data sources many other frameworks
- Distribute your data and processing across resizable clusters of Amazon EC2, EKS. Also has serverless option. Bigdata frameworks          Compute engines
- Auto-scaling and integrated with EC2 Spot instances. EC2
- Uses HDFS (EBS/Instance store) and EMRFS (S3) for storage.
- Use Cases: Log analysis, Web indexing, Processing data for machine                            EMR                  EKS learning training, financial data analysis, market trends, customer                                            Serverless preferences etc.
Data Targets S3        Redshift


## AWS Glue vs Amazon EMR
*(Slide 690)*

AWS Glue                                                  Amazon EMR
- Primarily for running serverless ETL jobs, schema
- Primarily for Big data processing and ETL discovery and data catalogs
- Flexibility to run any big data framework i.e.
- Simplicity – Low code/no code (supports Spark,                     Spark, Hadoop, Hive, HBase, Flink, Presto etc. python and Ray engine)
- Suitable for large scale distributed data
- Suitable for ad-hoc & small batch jobs                             processing with consistent usage
- Fully managed and Serverless
- Some level of infrastructure management
- When to use AWS Glue?
- When to use Amazon EMR?
- Visual and low code ETL development tools
- Hadoop Migration from on-prem
- Need built-in capabilities like data source/target
- Have expertise beyond just Spark, for ex. Hive, connectors, transformations, incremental load, job             Presto monitoring, orchestration.
  - Customer is skilled in loading their own data
  - Need centralized data catalogs and governance                  source connector libraries for their jobs.


## Amazon Athena
*(Slide 691)*


## Amazon Athena
*(Slide 692)*

- Provides the easiest way to run ad hoc interactive SQL queries for data in Amazon S3 and other data sources without the need to setup or manage any servers.
- Athena is serverless, built on open-source Trino and Presto engines
- Pay only for the queries you run. Pricing: $5.00 per TB of data scanned.
- Supports CSV, JSON, or columnar data formats such as Apache Parquet and Apache ORC.
- Athena integrates with Amazon QuickSight for easy data visualization.
- Use cases: Ad-hoc SQL queries for Business intelligence and analytics, query AWS logs e.g. VPC Flow Logs, ELB Logs, CloudTrail logs
Query data Load Data               Access data
S3 Bucket                 Amazon Athena                QuickSight


## Data scanned when NOT partitioned                   Data scanned when partitioned
*(Slide 693)*

```text
        /reviews/products/product-X-reviews-2025-11            /reviews/products/X/2025/11/ → 1GB
        /reviews/products/product-X-reviews-2025-12            /reviews/products/X/2025/12/ → 1GB
        /reviews/products/product-X-reviews-2026-01
                                                      → 6 GB   /reviews/products/X/2026/01/ → 1 GB
        /reviews/products/product-Y-reviews-2025-11
                                                               /reviews/products/Y/2025/11/ → 1GB
        /reviews/products/product-Y-reviews-2025-12
                                                               /reviews/products/Y/2025/12/ → 1GB
        /reviews/products/product-Y-reviews-2026-01
                                                               /reviews/products/Y/2026/01/ → 1GB
```


## Athena Federated queries
*(Slide 694)*

- If you have data in sources other than Amazon S3, you can use Athena Federated Query to query the data
- Supports data sources such as DynamoDB, RDS, CloudWatch Logs, and custom data sources including on- premises databases.
- Uses Data Source Connectors that run on AWS Lambda
- Query results are written to Amazon S3


## Athena for querying AWS services logs
*(Slide 695)*

- We can use Athena to query logs generated by various AWS services such as AWS CloudTrail, Application load balancer access logs, S3 access logs, Network firewall logs, VPC flow logs and many more..
Event logs
CloudTrail SELECT useridentity.username, sourceipaddress, eventtime, additionaleventdata                                                          Access logs FROM your_athena_tablename WHERE eventname = 'ConsoleLogin' Athena AND eventtime >= ‘2026-01-15T00:00:00Z'                                                         ELB AND eventtime < ‘2026-01-17T00:00:00Z'; Flow logs Example: CloudTrail event logs to query AWS console login events
VPC
More..


## Amazon QuickSight
*(Slide 696)*


## Amazon QuickSight
*(Slide 697)*

Data Sources
- Serverless machine learning-powered Business Intelligence service to create interactive dashboards. Redshift    S3    OpenSearch
- Fast, automatically scalable, embeddable, with per-session pricing.                                                       csv Amazon
- Integrates with Amazon RDS, Aurora, Athena, S3, OpenSearch,                                  QuickSight Redshift as data source
- Allows uploading files (CSV, XLS, JSON etc.) using file data source
Use Cases: Build Visualizations, Perform ad-hoc analysis
Smart Visualizations and Dashboards


## Amazon QuickSight - Important to know
*(Slide 698)*

Amazon QuickSight SPICE
- SPICE is super fast, parallel and in-memory calculation engine designed to help accelerate QuickSight dashboard performance.
- No need to provision or manage infrastructure for SPICE.
- The data stored in a SPICE dataset is a snapshot of your source data.
- To fetch the latest data from the source and refresh a SPICE dataset, you can configure a schedule or use an event-driven approach.
Image source: https://aws.amazon.com/blogs/business-intelligence/best-practices-for- amazon-quicksight-spice-and-direct-query-mode/


## Amazon QuickSight – Important to know
*(Slide 699)*

Data set: Users & Groups                                                                           Region    Revenue
- Amazon QuickSight has its own users and groups                                    APAC       500
- These users are different than AWS IAM users EMEA       300
- You can create User Groups (Enterprise edition) NA         450 Sharing analysis and dashboard APAC       700
- You can share analyses or dashboards with individual users or groups. RLS Rule:
- Users who receive a shared dashboard can view and interact with the results but cannot edit the analysis.                                                        User          Region Data Access and Security                                                               Alice      EMEA
- Users can view visualized data in dashboards, but do not have direct access to    Bob        APAC the raw underlying data sources. Carol      NA
- Supports Row-Level Security (RLS) in Enterprise edition. Dirk       EMEA, APAC


## Demo – Data pipeline using S3, Glue, Athena
*(Slide 700)*

```text
                                                                                                              High level steps

                                        3                                                      1   Upload sample CSV file to S3
             Glue ETL
                Job                                                                                Create AWS Glue crawler and run it. This
                                                                                               2
                                                                                                   should create a data catalog with Glue
                                              AWS Glue            Glue Data                        database and table
                                                                   Catalog
                                               2   4
                                                     Glue                                      3
                                                                                                   Create Glue ETL (python) job to modify
                                                    crawler                                        one of the column in the CSV and create a
                                                                                                   new CSV file in S3

                                                                                                   Rerun the Glue crawler again. This should
                                                                                               4
                                                                                                   create another table in the database
                              1                               5               6
       Employee data                                                Amazon         Amazon
                                                                                               5   Using Amazon Athena query the Glue
                                                                    Athena        QuickSight
                                                                                                   database and table created above


                                                                                               6   (Optionally) Create analysis and
                                                                                                   dashboard in Amazon QuickSight using
                                                                                                   Athena as a data source
```


## Amazon Redshift
*(Slide 701)*


## Web
*(Slide 702)*

```text
                                                        Users                                 Browser
         myapp.com on AWS                                                                                                     CloudFront

                                                         Route53            myapp.com                                                             Edge
                                                                                                                                                Locations



                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                     Lambda

                                                Web                                                                             Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                               Convert
                                                                    S           S                Rekognition             S3                S3
                            SNS
                                                                                                           AI models                                    QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker
                                                                                                                                                  Data
                                                                                                                                                Warehouse
                            SQS
                                              ElastiCache

                                                                               Neptune        Kinesis                    S3              EMR
                                                                                                                                                            Redshift
                       CloudWatch                  Multi-AZ




                                                              RDS       DynamoDB              Glue
```


## Amazon Redshift
*(Slide 703)*

- Amazon Redshift is a fully managed, petabyte-scale data warehouse service Data
- Amazon Redshift uses massively Parallel Processing (MPP) architecture and has a SQL interface for performing the queries.
- Can ingest data across data lakes, databases, streaming data - with no code/low code zero-ETL approach
- BI tools such as Amazon QuickSight, Tableau work seamlessly with Redshift. Amazon
- Allows sharing data across AWS regions, teams, and third-party data warehouses without             Redshift data movement or data copying.
- Redshift Cluster options:
  - Redshift Serverless
  - Provisioned Cluster
Analyze


## Redshift Serverless vs Provisioned Cluster
*(Slide 704)*

Redshift Serverless
- No cluster to manage. Fast setup, minimal admin effort
- Uses RPU (Redshift Processing Units) to scale up/down the CPU, memory and network
- Best for sporadic, unpredictable workloads
Redshift Cluster
- There is a leader node and one or more compute nodes
- Leader node distribute the query across compute nodes
- You can choose node type and node count
- Manual and scheduled scaling
- Best for steady, predictable workloads


## Redshift Spectrum
*(Slide 705)*

- Allows Amazon Redshift to query data directly in Amazon S3 without loading it
- Automatically scales compute independently of the Redshift cluster
- Best for querying large, infrequently accessed data (data lake)
- Enables separation of storage and compute


## Loading data from S3 into Redshift
*(Slide 706)*

- COPY command
COPY customer FROM 's3://amzn-s3-demo-bucket/mydata'           S3 IAM_ROLE 'arn:aws:iam::0123456789012:role/MyRedshiftRole';
COPY
  - Auto copy Jobs using S3 Event integration
COPY public.target_table FROM 's3://amzn-s3-demo- bucket/staging-folder' IAM_ROLE 'arn:aws:iam::123456789012:role/MyLoadRoleName' JOB CREATE my_copy_job_name AUTO ON; Redshift


## Amazon Streaming data services
*(Slide 707)*

```text
                 Sources                                                                              Destinations
                                               Streaming data   Streaming data       Streaming data
                                                  ingestion     processing and           delivery
                                                                   analytics
              Website Click
               Streams                                                                                 Amazon S3


                                              Amazon Kinesis
                IoT Devices                    Data Streams
                                                                                                      Amazon Redshift
                                                                Amazon Managed       Amazon Data
                                                                Service for Apache     Firehose
                                                                   Flink (MSF)
                Banking                       Amazon Managed                                             Amazon
              Transactions                     Streaming for                                            OpenSearch
                                               Apache Kafka
                                                   (MSK)          EC2    Lambda

            Metrics and Logs
                                                                   ECS    EKS
```


## Amazon Kinesis
*(Slide 708)*


## Amazon Streaming data services
*(Slide 709)*

```text
                 Sources                                                                              Destinations
                                               Streaming data   Streaming data       Streaming data
                                                  ingestion     processing and           delivery
                                                                   analytics
              Website Click
               Streams                                                                                 Amazon S3


                                              Amazon Kinesis
                IoT Devices                    Data Streams
                                                                                                      Amazon Redshift
                                                                Amazon Managed       Amazon Data
                                                                Service for Apache     Firehose
                                                                   Flink (MSF)
                Banking                       Amazon Managed                                             Amazon
              Transactions                     Streaming for                                            OpenSearch
                                               Apache Kafka
                                                   (MSK)          EC2    Lambda

            Metrics and Logs
                                                                   ECS    EKS
```


## Amazon Kinesis
*(Slide 710)*

- Amazon Kinesis Data Streams: A managed service to ingest real-time streaming data such as logs, metrics, and events.
- Amazon Kinesis Video Streams: A managed service to securely ingest, store, and process real- time video and media streams from devices and cameras.


## Kinesis Data                                  Producers                               Consumers
*(Slide 711)*

```text
        Stream                                            KPL                                     KCL
                                                                        Kinesis Data Stream
                                                     Kinesis Producer                          Kinesis Client
                                                          Library                                 Library
                                                                              shard


                                                                               shard
                                              EC2   AWS SDK for Java                          AWS SDK for Java   EC2

                                                                               shard
                                                         Agent
                                                      Kinesis Agent                            AWS Lambda




                                                      AWS Services                             Amazon Data
  DynamoDB Database CloudWatch                                                                  Firehose
                                                                               shard
   Streams Activity Stream Logs


                                                      3rd party tools                         Amazon Managed
                                                                                                 Service for
                                                                                                Apache Flink
```


## Kinesis Data Stream - Shards
*(Slide 712)*

Kinesis Data Stream
- Kinesis Data Streams comprise of Shards
- Each shard provides maximum read data rate of     Producers                  shard             Consumers 2MB/sec and write data rate of 1MB/sec (or 1000 records/sec)                                                 1MB/sec          shard      2MB/sec
- A partition key is used to group the records by shard within a stream ensuring ordering of                                    shard KPL                                          KCL records within a shard Kinesis Producer                             Kinesis Client
- Number of shards decides the capacity of the           Library                                    Library stream.
- Data in stream can be retained for up to 365 days (24 hrs is default retention period)                                     shard
- Data can’t be deleted (until expires)
- Data can be replayed and reprocessed by the consumers (if required)


## Kinesis Data Stream – Capacity modes
*(Slide 713)*

Provisioned Mode
- You explicitly define the number of shards in the stream shard
- Each shard provides 1 MB/sec write (1,000 records/sec) and 2 MB/sec read throughput                                                                         shard
- Scaling requires manual shard split or merge operations shard
- Best suited for predictable and steady streaming workloads On-demand Mode
- Automatically scales read and write throughput based on observed traffic over the last 30 days
- No shard capacity planning or shard management required shard
- Ideal for unpredictable or highly variable workloads Amazon Kinesis Data Stream


## Amazon Kinesis – Use cases
*(Slide 714)*

- Fraud detection in Financial services
- Analyzing customer behavior in real-time – personalized recommendations, offers, discount codes
- Logs monitoring and alerting
- Read time Ad targeting Amazon Kinesis
- IoT Data processing – To detect malfunctions or anomalies                             Data Streams
- Social Media – Trending topics, user engagement, content recommendations
- Home security and Smart cameras – Motion detection, face detection
- Connected vehicles – Dizzy driver, Drive Behavior
- Industrial IoT and remote monitoring – Equipment health, Oil rigs in powerplant, security Amazon Kinesis breaches                                                                               Video Streams


## Amazon MSK
*(Slide 715)*

- Fully managed Apache Kafka service on AWS (alternative to Amazon Kinesis Data Stream)
- Used for real-time streaming ingestion and messaging
- AWS manages brokers, Zookeeper/KRaft, patching, and scaling
- Compatible with open-source Kafka APIs and tools Amazon Managed Streaming
- Data is stored across Kafka topics and partitions                                   for Apache Kafka (Amazon MSK)
- Provides high availability using multi-AZ clusters, offers Serverless option.
- Integrates with IAM, TLS, and encryption at rest
- Commonly used with Apache Flink, Lambda, Spark, EC2 consumers


## Amazon Managed Service for Apache Flink (MSF)
*(Slide 716)*

- Fully managed service to run Apache Flink for real-time Kinesis Data stream processing Stream
- Common sources include Kinesis Data Streams and Amazon MSK
- You define input sources and output sinks                                                     Firehose stream Kinesis Data Use cases Stream
- Real-time aggregations - Rolling counts, sums, averages                                        Kafka Topic over time windows                                                         Amazon Managed Service
- Stream data enrichment - Join live events with reference   Amazon MSK      for Apache Flink data from databases
- Anomaly detection & alerts - Detect patterns like fraud, detect anomalies when thresholds are breached


## Amazon Managed Service for Apache Flink (MSF)
*(Slide 717)*

- Fully managed service to run Apache Flink for real-time Kinesis Data stream processing Stream
- Common sources include Kinesis Data Streams and Amazon MSK
- You define input sources and output sinks                                                     Firehose stream Kinesis Data Use cases Stream
- Real-time aggregations - Rolling counts, sums, averages                                        Kafka Topic over time windows                                                         Amazon Managed Service
- Stream data enrichment - Join live events with reference   Amazon MSK      for Apache Flink data from databases
- Anomaly detection & alerts - Detect patterns like fraud, detect anomalies when thresholds are breached


## Amazon Data Firehose
*(Slide 718)*

(Formerly Kinesis Data Firehose)
Transform Amazon S3    Amazon Agent                                                                    OpenSearch Kinesis          Kinesis                                                  COPY AWS IoT             Agent              Data Streams S3 Bucket           Amazon Redshift
CloudWatch                                               Firehose Delivery Amazon MSF                         Stream Logs and events
- Failed deliveries
- Failed Transformation                3rd Party SaaS platforms
- All source records (backup)
HTTP endpoints


## Amazon Data Firehose
*(Slide 719)*

- Delivers streaming data to destinations such as Amazon S3, Amazon Redshift, Amazon OpenSearch Service, and HTTP endpoints Lambda
- Integrates with third-party destinations such as Splunk and Datadog
- Supports configurations like buffer size and buffer time to control speed and size of records
- Supports formats like JSON, CSV, Parquet, ORC, raw text, and binary
- Can convert data formats (e.g., JSON → Parquet / ORC) before delivery (built-in feature)                                                                                Amazon Data Firehose
- Supports compression (GZIP, Snappy, ZIP) to reduce storage cost
- Allows inline data transformation using AWS Lambda before delivery


## Amazon Data Firehose
*(Slide 720)*

Use cases:
- Ingest application logs and deliver them to Amazon S3 for long-term storage and analytics
- Send Clickstream or event data directly into Amazon Redshift for near–real-time reporting
- Send operational logs to Amazon OpenSearch Service for search and visualization
- Deliver metrics and traces to third-party tools like Splunk JSON or Datadog                                                           Amazon Data   Amazon S3 Firehose
- Convert and compress streaming data (e.g., JSON → Parquet + GZIP) before landing in S3


## AWS Lake Formation
*(Slide 721)*


## Access S3 data directly
*(Slide 722)*

```text
       {
        "Sid": "ReadObjects",
        "Effect": "Allow",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::example-bucket/*"
       },




                      IAM Policy




                               Data User              Amazon S3
```


## Access S3 data using Athena
*(Slide 723)*

```text
       {
        "Sid": "ReadObjects",
        "Effect": "Allow",
        "Action": "s3:GetObject",                       Glue Data
        "Resource": "arn:aws:s3:::example-bucket/*"
       },                                                Catalog
     {
           "Sid": "AthenaQueryAccess",
           "Effect":IAM  Policy
                     "Allow",
           "Action": [
             "athena:StartQueryExecution",
             "athena:GetQueryExecution",
             "athena:GetQueryResults"
           ],
             "Resource": "*"
           },             Data User                   Amazon Athena   Amazon S3
           {
            "Sid": "GlueCatalogRead",
            "Effect": "Allow",
            "Action": [
               "glue:GetDatabase",
               "glue:GetTable",
               "glue:GetPartitions"
           ],
            "Resource": "*“
     }
```


## Access S3 data using Athena
*(Slide 724)*

- S3 read permissions
- Athena execution permissions     Glue Data
- Glue catalog permissions          Catalog
IAM Policy
Data User             Amazon Athena   Amazon S3
Permissions are S3 Object level


## Access S3 data using Lake formation permissions
*(Slide 725)*

Set up permissions for databases, tables, columns and rows
- S3 read permissions
- Athena execution permissions     Glue Data      AWS Lake                        Data lake Formation                      Administrator
- Glue catalog permissions          Catalog
IAM Policy
Data User             Amazon Athena   Amazon S3


## Access S3 data
*(Slide 726)*

Set up permissions for databases, tables, columns and rows
- Athena execution permissions     Glue Data                            AWS Lake                       Data lake Catalog                             Formation                     Administrator
- Glue catalog permissions
Get temp credentials IAM Policy
Data User             Amazon Athena                         Amazon S3
Permissions are database, table, row or column level


## AWS Lake Formation
*(Slide 727)*

- Provides centralized, fine-grained data access control at table, column, and row level Data lake Administrator
- Builds on the AWS Glue Data Catalog to store metadata and enforce permissions                                                                                             Check 3      Permissions
- Supports resource-based permissions (e.g. grant                               2 Get metadata SELECT on a specific database or table such as company.finance)                                                                                Glue Data Catalog AWS Lake
- Supports attribute-based access control (ABAC) using                                                                                Formation 4 Get credentials LF-tags (key–value labels like department=finance) 5 Access data
- Without Lake Formation:                                                        Athena
  - Users require IAM permissions to directly access data in   (or any other engine) Amazon S3 1
- With Lake Formation:
  - Users require Lake Formation permissions, and direct S3 access is not needed for querying data
Data User


## AWS Lake Formation for Data Lake setup
*(Slide 728)*

Automates Data import using Lake Formation Workflows and Blueprints
- A workflow uses a blueprint to define and automate the steps required to ingest data into AWS Lake Formation
- This includes creating the data lake structure, running crawlers, and registering data locations.
- Blueprints take the data source, data target, and schedule as input to configure the workflow.
- AWS provides blueprints for sources like Relational databases, CloudTrail logs, NoSQL databases, S3 etc.


# Machine Learning and AI

*(Source: Slide 729)*

### AWS Machine Learning & AI


## AWS’ three-layered approach to AI/ML
*(Slide 730)*

```text
                                                                 Artificial Intelligence (AI) Services
 Users with no-prior




                                                                                                                                                                Fully managed AI
  ML experience




                         Vision               Speech                                    Text                           Search           Contact Center




                                                                                                                                                                     services
                        Amazon                     Amazon                 Amazon     Amazon         Amazon             Amazon              Amazon
                                        Amazon
                       Rekognition                Transcribe              Textract   Translate    Comprehend           Kendra              Connect
                                         Polly


                                                                      ML Platform and services
and Developers




                                                                                                                                                                Heavy lifting by
Data Scientists




                                                                                                                                                                    AWS
                                 Amazon                        Notebook       Training           Canvas        Model   Shadow testing Geospatial ML
                               SageMaker AI

                                                                 Infrastructure and ML frameworks
ML Experts and




                                                                                                                                                                customization
                                                                                                                                                                 Flexibility &
 Practitioner




                                     CPUs                                        Deep Learning
                                     GPUs                       EFA              AMIs

                                     Inferentia                                  Containers
```


## AWS AI services
*(Slide 731)*


## Using AWS AI services
*(Slide 732)*

```text
                                              API call   Send Audio, Video, Images, Text



                                                          Receive response
                                                                                           AWS AI Services
```


## Amazon Rekognition
*(Slide 733)*

  - Computer Vision based AI service to find people, texts, objects, scene in images and videos.
  - Common use cases:
    - Celebrity recognition
    - Face compare and search
    - Face detection and analysis
    - Content moderation
    - Detect objects, Brand logos, Texts
    - Video segment detection (blank frames etc.)
    - Easy filtering of video for explicit and suggestive content
- https://aws.amazon.com/rekognition/


## Web
*(Slide 734)*

```text
                                                        Users                                 Browser
                                                                                                                              CloudFront
                       myapp.com on AWS
                                                         Route53            myapp.com                                                             Edge
                                                                                                                                                Locations



                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                     Lambda

                                                Web                                                                             Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                               Convert
                                                                    S           S                Rekognition             S3                S3
                            SNS
                                                                                                           AI models                                    QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker


                            SQS
                                              ElastiCache

                                                                               Neptune        Kinesis                    S3              EMR
                                                                                                                                                            Redshift
                       CloudWatch                  Multi-AZ




                                                              RDS       DynamoDB              Glue
```


## Amazon Polly
*(Slide 735)*

- Converts text to speech
- Create applications that talk to increase engagement and accessibility
- Supports Speech synthesis - phrases, punctuations, pauses
- Common use cases:
  - Add speech for global audience RSS feeds, websites, blogpost                                     “Press blue
  - Automated voice response system (“Dear customer,        button to your account balance is zero ☺”)                       turn on the Car A.C.”
  - Supports variety of lifelike voices that you can choose from to better serve your customers as per their location                                             Text        Audio


## Amazon Transcribe
*(Slide 736)*

  - Automatic speech recognition (ASR) service to convert audio to text.
  - Supports Automatic Language Identification
  - Supports over 100+ languages (English, Chinese, Arabic, French, Korean, Hindi, German etc.)
  - Automatically removes Personally Identifiable Information (PII) using redaction.
  - Multiple Speakers and channels identification
  - Custom Vocabulary                                                                        “Hello ! I
- Common use cases:                                                                          hope you started ✓ Transcribe customer service calls                                                       loving AWS ✓ Automate closed captioning and subtitling                                                 by now” ✓ Generate metadata for media assets to create a fully searchable archive Audio                               Text ✓ Detect toxic content in the audio (social media) Amazon Transcribe for medical to convert clinical conversation into health records


## Amazon Textract
*(Slide 737)*

- Automatically extract printed text, handwriting, layout elements, and data from scanned documents.
- Much powerful than simple OCR (optical character recognition) where it understand the document layout, forms, tables, images and extracts relevant and related data
- Use Cases:
- Financial Services (e.g., Invoices, Financial reports)
- Healthcare (e.g., Medical records, Insurance claims)
- Public Sector (e.g., Tax forms, ID documents, Passports)
{ “Document ID”: “P777777777777”, Analyse                        Result “Name”: “JANE A SAMPLE”, “SEX”: “F”, “DOB”: “01-01-83”, Amazon Textract … }


## Amazon Translate
*(Slide 738)*

- Fluent and accurate language translation.
- Supports translating text between 75 languages (& growing)
- Use cases: Translate user manuals, books, documents, websites etc.


## Amazon Comprehend
*(Slide 739)*

- Uses Natural Language Processing (NLP) to extract insights about the content of documents.
- Finds insights and relationships in text such as:
  - Language of the text
  - Extracts key phrases, places, people, brands, or events
  - Understands how positive or negative sentiments are
  - Automatically organizes a collection of text files by topic Amazon Comprehend                             Analyse and extract Use Cases:
  - Analyze customer interactions to find what leads to a positive or negative experience
  - Create and groups articles by topics
Key Phrases
Sentiments
Language Entities
Topics Amazon Comprehend Medical - Extract medical information from medical text like doctors’ notes, clinical trial reports, or radiology reports


## Amazon Kendra
*(Slide 740)*

- An intelligent ML powered enterprise search service
- Employees and customers can find the content they’re looking for from multiple locations and content repositories within your organization.
- Indexes variety of documents such as Websites/HTML, Share point, PPT, MS Word and provides accurate answers based on Natural language search capabilities.
- Incremental learning from user feedback
S3 Bucket Web Crawler               RDS     Confluence Connectors                              How to raise IT ticket Indexing                     for new laptop?
Dropbox             JIRA            Slack       OneDrive                           Amazon Kendra 180+ connectors


## Amazon Connect
*(Slide 741)*

- AI-powered cloud contact center
- Automatically detects customer issues and provides agents with contextual customer information and suggested responses and actions for faster resolution of issues.
- Easy to create flows and integration with other CRM systems or AWS
Call             Stream          Invoke               Schedule
Phone call to                 Connect            LEX               Lambda                CRM Schedule an Appointment for a service


## Amazon Connect
*(Slide 742)*

- AI-powered cloud contact center
- Automatically detects customer issues and provides agents with contextual customer information and suggested responses and actions for faster resolution of issues.
- Easy to create flows and integration with other CRM systems or AWS
Agent Workspace:
- When an agent accepts a call, chat, or task, they receive necessary information about the case and customer and real-time recommendations.
- 80% cheaper than traditional contact center solutions.


## Amazon SageMaker AI
*(Slide 743)*


## Machine Learning with Amazon SageMaker AI
*(Slide 744)*

```text
                                                                                      Feature engg. / hyperparameter tuning

                                                                                                Test data
       square                                                                                                                             NOT OK
                         triangle triangle       circle
                                                                           Develop
                circle

  triangle                           star                                     ML model
                            cube                      circle
                                                                                                                              Prediction Confidence




                                                                                                     Prediction
                heart
                                              heart       cube
                               square                                                                                           square    95%
       square
                                         triangle triangle
                 triangle                                                                                         Result                           OK
                                                                                                                               triangle   99%
   star circle                      cube
                                                heart
                          circle                                                               ML Model
                                                                                               (Trained)                       square     47%
                                                                      ML Model
      cube                         square        star
                                                                      Training
                rectangle                                                                                                                 NOT OK
                                                                              Data augmentation

                     Labelled Data
                                                                        Inference request                                  Deploy Model
                                                               Apps
                                                                              star
                                                                                                                                Supervised Learning
```


## Amazon SageMaker AI
*(Slide 745)*

- Fully managed service for developers / data scientists to build, train and deploy ML models at scale
- Amazon SageMaker Features:
  - SageMaker Studio – A web-based IDE interface for building, training and deploying models.
  - AutoML (SageMaker Autopilot) - Automatically explores and creates the best ML models
  - Built-in Algorithms – A wide range of built-in machine learning algorithms optimized for performance and scalability
  - Notebook Instances - Managed Jupyter notebooks with pre-installed libraries, scalable compute, and data storage, making it easier to explore data and develop models.
  - Training & Tuning – Manages ML training infrastructure, Distributed training across multiple GPUs.
  - Model deployment – Deploy models to production in One click by creating endpointst, multi-model endpoints
  - SageMaker GroundTruth – A data labelling service (Text, images, video labelling)
  - SageMaker Model Monitor - Continuously monitors the quality of your models in production, detecting drift in model performance.
  - SageMaker Pipelines: Facilitates the automation and orchestration of machine learning workflows, supporting the implementation of MLOps best practices.


# AWS Edge Networking

*(Source: Slide 746)*

*Access applications with lowest latency across the globe*


## Amazon CloudFront
*(Slide 747)*


## Web
*(Slide 748)*

```text
                                                        Users                                 Browser
                                                                                                                              CloudFront
                       myapp.com on AWS
                                                         Route53            myapp.com                                                             Edge
                                                                                                                                                Locations



                                                                         ELB
                                                                                     Auto
                                                                                    Scaling                                     Lambda

                                                Web                                                                             Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                               Convert
                                                                    S           S                Rekognition             S3                S3
                            SNS
                                                                                                           AI models                                    QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker


                            SQS
                                              ElastiCache

                                                                               Neptune        Kinesis                    S3              EMR
                                                                                                                                                            Redshift
                       CloudWatch                  Multi-AZ




                                                              RDS       DynamoDB              Glue
```


## Amazon CloudFront
*(Slide 749)*

- Amazon CloudFront is a global Content Delivery Network (CDN) service that delivers content with low latency through AWS edge locations. myapp.com
- CloudFront operates with 100+ Points of Presence and 10+ Regional Edge Caches (RECs) globally.                                                          HTTPS/TLS
- Regional Edge Caches act as a mid-tier caching layer between the origin and edge locations. They store objects longer than edge locations.
- Built-in DDoS protection with Amazon Shield (Layer 3 and 4) and Web attack                        Route 53 protection with AWS WAF (Layer 7).
- Integrates with Amazon Certificate Manager (ACM) to use SSL/TLS certificates for secure HTTPS connections.
- Integrates with Amazon Route 53 (DNS) which allows you to use your own Amazon CloudFront custom domain name with the CloudFront distribution.


## CloudFront
*(Slide 750)*

```text
                                                                               Origin Servers
                                                 Cached           Cached
                                                                               AWS Cloud




                 Viewers




                                                                               Corporate
                                                                               data center

                 Viewers

                                                               Regional Edge
                                              Edge Locations
                                                                  Caches
```


## How it works?
*(Slide 751)*

```text
                                              AWS Cloud


                                               Public DNS




                                               Application
                                              Load Balancer
```


## How it works?
*(Slide 752)*

```text
                                                                        AWS Cloud


                                                                         Public DNS




                                                                         Application
                                                                        Load Balancer




                                              CloudFront Distribution
```


## Amazon CloudFront Origins
*(Slide 753)*

- S3 Bucket                                                                                                   AWS Lambda S3 bucket                 (Function URL)
- Custom HTTP Origins (Public)
  - EC2                                                                                            EC2 Instance (Public or EIP)
CloudFront Origins
  - ALB Application
  - API gateway                                                           Custom Origin               Load Balancer
  - Lambda Function URL                                                  (HTTP/HTTPS)                 (Public DNS) API Gateway
  - Any HTTP endpoint      Amazon                                                                     (Public DNS) CloudFront
- VPC Origin (Private)                                                                                 Any HTTP/S endpoint
  - EC2
  - ALB Private subnet
  - NLB
VPC Origin ENI        EC2   ALB      NLB


## CloudFront – Multiple Origins
*(Slide 754)*

- CloudFront can route viewer requests to different origins based on request attributes. Cache Behaviors   Origins
- A common method is path-based routing, where the URL path determines which origin should                                        /api/* handle the request.
- Example:                                                                           /auth/*
  - /api/* → API Gateway origin ALB
  - /auth/* → Application Load Balancer origin   Amazon CloudFront
  - /images/* → S3 origin /images/*
- CloudFront achieves this using Cache Behaviors S3 Bucket for different origins


## S3 Origin
*(Slide 755)*

- S3 is a popular CloudFront origin because large applications store terabytes of static media (e.g. images, videos) in S3, and CloudFront delivers it faster through edge network and caching it closer to the end users.
- Reduced Data transfer out rate as compared to internet (+1TB free data transfer out per month)
- You can block direct access to S3 using CloudFront Origin Access Identity (OAI) or Origin Access Control (OAC)​​
Cached
AWS Backbone network
Amazon                                            S3 bucket CloudFront                                   (as CloudFront Origin)


## Block direct access to S3
*(Slide 756)*

CloudFront Origin Access Identity (OAI)                          CloudFront Origin Access Control (OAC)
- Legacy mechanism to access a private S3
- New mechanism to access a private bucket.                                                       S3 bucket.
- Creates IAM-like identity
- Uses SigV4 signing S3 Bucket Policy for OAI   {                            S3 Bucket Policy for OAC "Version": "2012-10-17", {                                                           "Statement": [ "Version": "2012-10-17",                                    { "Statement": [                                                "Sid": "AllowCloudFrontOAC", {                                                           "Effect": "Allow", "Sid": "AllowCloudFrontAccess",                           "Principal": { "Effect": "Allow",                                           "Service": "cloudfront.amazonaws.com" "Principal": {                                            }, "AWS":                                                 "Action": "s3:GetObject", "arn:aws:iam::cloudfront:user/CloudFront                        "Resource": "arn:aws:s3:::<bucket-name>/*", Origin Access Identity <OAI-ID>"                                "Condition": { },                                                           "StringEquals": { "Action": [                                                    "AWS:SourceArn": "s3:GetObject"                                   "arn:aws:cloudfront::<account- ],                                                  id>:distribution/<distribution-id>"                           S3 bucket "Resource": "arn:aws:s3:::<bucket-                           }                                              (as CloudFront Origin) name>/*"                                                        } }                                                         } ]                                                         ] }                                                         }


## Origin Access Control (OAC)
*(Slide 757)*

- Restricts direct access to your S3 bucket, ensuring content is only accessible through CloudFront.
- OAC is the modern, more secure replacement for Origin Access Identity (OAI is legacy).
- OAC comes with Enhanced security with AWS Signature Version 4 (SigV4). OAI uses Legacy (SigV2) for Authentication
- OAC supports SSE-KMS encryption (OAI does not)
- Note: You cannot use OAC (or OAI) with an S3 bucket configured as a Static website endpoint. For website endpoints, you must treat the S3 bucket URL as a Custom Origin.


## Exercise: CloudFront with S3 Origin
*(Slide 758)*

1. Create an S3 bucket and upload a sample image (keep bucket private).
2. Try accessing image with the S3 URL -> Access Denied
3. Create a CloudFront distribution and choose the S3 bucket as the origin. OAC Signing   4.   Configure Settings – Allow private S3 bucket access to CloudFront - Recommended
5. Let CloudFront update the S3 bucket policy automatically to allow only CloudFront (via OAC) to access the bucket.
6. Deploy the CloudFront distribution and wait for propagation.
7. Access the image with CloudFront distribution DNS, it should work S3 bucket (as CloudFront Origin)


## Amazon CloudFront – Security features
*(Slide 759)*

- HTTPS
- Mutual TLS (mTLS)
- CloudFront Field-level encryption
- Singed Cookies and Signed URL
- Restricting VPC based Origins access from CloudFront using Security Groups                                              Amazon CloudFront
- AWS WAF and AWS Shield for CloudFront


## HTTPS / TLS
*(Slide 760)*

- CloudFront supports HTTPS between Viewer and CloudFront                                Viewer
- Viewer Protocol Policy:
  - Allow HTTP & HTTPS                                                      HTTPS/TLS
  - Redirect HTTP to HTTPS
  - HTTPS only
- CloudFront supports HTTPS between CloudFront and Origin                                     Amazon CloudFront
- Origin Protocol Policy:
  - HTTPS only
  - Match viewer HTTPS/TLS
  - HTTP only
- You can enforce minimum TLS versions (e.g. TLS1.2) using Security Policies. CloudFront
- For using custom domain name (e.g. example.com) you need to have SSL/TLS        Origins Certificate in ACM in us-east-1 region


## Mutual TLS (mTLS)
*(Slide 761)*

mTLS = Client certificate authentication at CloudFront                                                         Viewer
- Viewer presents a client certificate during TLS handshake. HTTPS/TLS
- CloudFront validates that certificate against a trusted CA stored in ACM.
- CloudFront verifies:
  - Certificate chain → must chain to the trusted CA in ACM
  - Certificate validity period → not expired, not before valid
  - Revocation supported only when using AWS Private CA (via CRL/OCSP        AWS Private behind the scenes)                                                          CA
- Works only with custom domains, not with the default *.cloudfront.net                                    HTTPS/TLS domain.
- mTLS is enforced before CloudFront routes the request to the origin.             If CloudFront client cert fails, CloudFront returns 403 Forbidden.                                                  Origins


## CloudFront’s field-level encryption (FLE)
*(Slide 762)*

- Adds an extra security layer on top of HTTPS by encrypting specific sensitive fields (e.g., credit card numbers, PII).
- Encryption happens at CloudFront edge, so protected fields stay encrypted through proxies, logs, and internal systems.
- Uses asymmetric encryption: CloudFront uses public key, origin uses private key to decrypt. (can use openssl to create keys)
- Ensures only the origin application can read sensitive values; CloudFront and intermediaries cannot.
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/field- level-encryption.html


## Serving Private content through CloudFront
*(Slide 763)*

```text
         Allow access to only authenticated Viewers


                                              Application

        Signed URL                                                                        S3 Bucket

       Signed Cookies                                              Trusted Key Group




                                              Validate signature




                                                                                       Other Origins
```


## Signed Cookies and Signed URL
*(Slide 764)*

- CloudFront Signed URLs / Signed Cookies are used to serve private content securely - content that should be accessible only to authorized users (e.g. paid users, subscribers, restricted documents/media).
- Signed URLs and Signed Cookies are designed for short-lived, temporary access - you specify an expiration time, so access automatically ends after a defined period.
- Signer has a public/private key pair. The private key is used to sign the URL or cookie; CloudFront uses the public key to verify validity.
- When a user requests a resource with a valid signature (URL or cookie), CloudFront verifies the signature + policy (e.g. expiration, allowed resource) and if valid, serves the content (from cache or origin) otherwise returns 403 Forbidden / Access Denied response.
- Use Signed-URLs to restrict access to a single file (e.g. download, shareable link).
- Use Signed-Cookies to restrict access to many files / streaming media / entire section.


## Restricting direct access to Origins
*(Slide 765)*

1. Restricting direct access from Viewers to Origin
OAC Signing
Custom Header
Other Origins


## Restricting origin access only from CloudFront
*(Slide 766)*

- Access to the origin can be restricted at network layer such that only CloudFront edge locations can reach it.
- This restriction can be applied only to origins that support Security Groups to whitelist (allow) CloudFront IP ranges.
  - ALB / NLB (inbound rules)
  - EC2 instances
  - VPC-hosted custom origins (ENIs)
- Two ways to get CloudFront’s origin-facing IP ranges:
  - AWS ip-ranges.json (needs automation) VPC
  - AWS Managed Prefix List (recommended) Security group
ELB or EC2


## Restricting origin access only from CloudFront
*(Slide 767)*

1. AWS ip-ranges.json
- The JSON file at https://ip-ranges.amazonaws.com/ip-ranges.json publishes all AWS public IP ranges.
- To find CloudFront origins, filter entries where:
  - "service": "CLOUDFRONT" and "region": "GLOBAL"
- These CIDRs can then be added as inbound Security Group rules.
- Requires automation (Lambda + EventBridge) because CloudFront IP ranges can change at any time hence
- Higher operational overhead.
2. AWS Managed Prefix List (recommended)                                              VPC
- AWS provides a managed prefix list: com.amazonaws.global.cloudfront.origin-facing                                Security group
- Automatically maintained by AWS — no manual updates required.
- Can be referenced directly in Security Group rules and route tables.
- Simplifies configuration and reduces the risk of missing updates.              ELB or EC2


## Using CloudFront Prefix list
*(Slide 768)*

```text
             Security group inbound rule:




                                              VPC

                                                    Security group



                                                      ELB or EC2
```


## AWS WAF and AWS Shield for CloudFront
*(Slide 769)*

- CloudFront integrates directly with AWS WAF to protect applications from common Layer 7 web attacks such as SQL injection, XSS, bad bots, and request floods.
- AWS WAF filtering happens before traffic reaches your origin, reducing load and preventing malicious requests from propagating.
- CloudFront includes AWS Shield Standard automatically, providing always-on AWS Shield protection against common Layer 3/4 DDoS attacks at no extra cost.
- For mission-critical workloads, AWS Shield Advanced gives enhanced DDoS protection, larger capacity, near real-time attack visibility, and 24/7 AWS DDoS Response Team (DRT) support.
- Using CloudFront + WAF + Shield together provides a multi-layered, global defense    AWS WAF against both network-layer and application-layer threats.


## AWS WAF and AWS Shield for CloudFront
*(Slide 770)*

```text
                                                         Sample Application architecture




                                               AWS
                                                                                     Application
                                               Shield
                                                                                    Load Balancer

                                                           AWS WAF
                                                                       CloudFront


                          X                    Shield
                                              Advanced
                                                                                    API Gateway
```


## CloudFront Features – Important to know
*(Slide 771)*

- CloudFront Geo Restriction
- CloudFront Cache Behavior - TTL and Cache Invalidation
- Serverless edge computing:
  - CloudFront Functions
  - Lambda@Edge Amazon CloudFront
- CloudFront with Custom Domain Name (Route 53)


## CloudFront Geo Restriction
*(Slide 772)*

- CloudFront lets you control which countries can access your content.
- CloudFront identifies the viewer’s country using a 3rd party trusted Geo-IP database.
- Allowlist (Whitelist): Only users from the approved countries can access your distribution.
- Blocklist (Blacklist): Users from the blocked countries are denied access.
- Common Use Case: Enforcing copyright rules, regional streaming restrictions, or limiting access to certain geographic regions.


## Cache Behaviour - TTL (Time To Live)
*(Slide 773)*

- TTL defines how long an object should be kept in the cache before fetching it from the origin if requested.
- Default TTL is 24 hrs. Can set Minimum and Maximum TTL.
- Lower TTL helps serve latest content but causes higher origin load
- Higher TTL provides better performance and lower cost but slower updates.
- Dynamic APIs usually set low/no caching and static assets set high TTL.


## TTL – Behind the scene
*(Slide 774)*

1. When the TTL for a file expires, and a new request for that file arrives, CloudFront contacts the origin                Send same                 Send new to check for a newer version                                         file from cache           file from cache
2. If the file on the origin has not changed, the origin returns an HTTP status code 304 Not Modified.
3. CloudFront then resets the TTL and serves the Reset TTL                  New file and existing cached file.                                                                           TTL
4. If the file on the origin has changed, the origin returns an HTTP status code 200 OK and sends the new version of the file. CloudFront replaces the                  304 - Not 200 - OK modified old cached file with the new one, resets the TTL, and serves the new file.
Content not changed            Content changed


## Cache Behaviour – Cache Invalidations
*(Slide 775)*

- Used to remove objects from CloudFront caches before TTL expiry. Invalidate
- After invalidation, CloudFront fetches the latest version from the origin.                        /venice.jpg
- Supports:
  - Specific object: /venice.png
  - Wildcard patterns: /images/*
  - All objects: *
- First 1,000 invalidation paths per month are free                            /venice.png         /venice.png
- Invalidation affects all edge locations globally.
- Common use cases:
  - Removing sensitive / incorrect files immediately /venice.png
  - Deploying new versions of static assets


## CloudFront - Serverless edge compute
*(Slide 776)*

- Brings compute closer to users by running code edge locations
- Serverless - automatically handles millions of requests per second.
- Ideal for lightweight, fast-running logic that needs to run for every request. Viewer              Viewer
- Can be applied at: Request            Response
  - Viewer Request
  - Origin Request
  - Origin Response
  - Viewer Response
- Offloads work from origins, reducing backend load, infrastructure cost, and improving Origin  application Origin performance.                                                                 Request        Response
- CloudFront offers: CloudFront Functions and Lambda@Edge.
Origin


## CloudFront Functions
*(Slide 777)*

- Lightweight JavaScript functions that run at CloudFront edge locations before cache lookup.
- Designed for ultra-low latency, massively scalable (tens of millions requests/sec)
- CloudFront Functions support two event triggers:
  - Viewer Request – before CloudFront checks the cache.                      Viewer              Viewer
  - Viewer Response – before CloudFront returns the response to the viewer.   Request            Response CF      CF
- Use cases – URL rewrites/redirect, Header manipulation
- Limitations
  - Supports code only in Java Script
  - No external network calls / APIs                                           Origin             Origin
  - No file system access, no environment variables                           Request            Response
  - Execution time lime < 1ms
  - Can not modify response body (Only headers/URI/status).
Origin


## Example - CloudFront Functions
*(Slide 778)*

Use case: Single-page apps (SPA) written in React/Vue etc. where every path should serve index.html How it works?
- On Viewer Request, rewrite requests that look like file-less routes (e.g., /products/123) to /index.html so the SPA can handle client-side routing.                                   Viewer              Viewer Request            Response
- Attached to Viewer Request. CF
- Sample Java Script CloudFront Function:
function handler(event) { var request = event.request;                                                                Origin             Origin var uri = request.uri;                                                                     Request            Response // if URI looks like a file (has a dot) let it pass if (uri.indexOf('.') !== -1) return request; // if root or a route, rewrite to index.html request.uri = '/index.html'; return request;                                                                                      Origin }


## CloudFront - Lambda@Edge
*(Slide 779)*

- Run Lambda functions at AWS CloudFront edge locations globally.
- Allows you to adjust data between the Viewer & Origin.
- Lambda@Edge supports all four event triggers.
- Currently supports Node.js and Python.                                              Viewer              Viewer Request            Response
- Function needs to be authored in N. Virginia (us-east-1) region
- Use cases:
  - Authentication and authorization checks by invoking external APIs
  - A/B testing, device-based or location-based routing.
  - Generate custom responses without calling the origin e.g. Site under    Origin             Origin maintenance                                                            Request            Response
  - Generating thumbnails or specific resolution of the image
Origin


## Example – Lambda@Edge
*(Slide 780)*

Use case: Image resizing How it works?
- User requests /images/photo.jpg?w=300&h=200 Viewer              Viewer
- CloudFront checks cache and if a cache miss, Lambda@Edge at Origin       Request            Response Request runs:
  - It fetches the original image from the origin
  - Run the code to generates the resized image
  - CloudFront caches the resized object. Origin             Origin
- Future requests for the same size are served directly from edge cache.   Request            Response
Origin


## CloudFront Functions vs Lambda@Edge
*(Slide 781)*

```text
                                                      CloudFront Functions                                 Lambda@Edge

                     Trigger Events             Viewer Request, Viewer Response              Viewer Request, Viewer Response, Origin
                                                                                                    Request, Origin Response
                    Execution Time                              < 1ms                           Typically, 10-50ms but can be more

                  Memory / Package                        Memory max2MB                               Memory - 128MB to 10GB
                       size                              Package size10 KB                           Package size – Up to 50MB
                         Runtime                           Java script only                              Node.js and Python

                       Complexity             Very small logic; no external libraries; no   Supports larger logic; can use libraries; can
                                                  network calls, no file system, no          call external systems, ephemeral storage
                                                      environment variables                    (/tmp) 512MB, environment variables
                      Performance                         Ultra-low latency                               Moderate latency

                            Cost                              Very low                               Higher as compared to CF

                        Use cases                Lightweight request/response                 More complex logic, dynamic responses,
                                               modification, URL rewrites, redirects,       origin routing, authentication, calling external
                                                header edits, simple auth checks              APIs, custom responses, HTML rewriting
```


## CloudFront - Alternate Domain Names
*(Slide 782)*

- You can use your own domain name(s) instead of the domain assigned by CloudFront to your distribution          www.example.com
- Example:
    - https://www.example.com/sample.jpg
    - https://api.example.com/profile/customer/101
    - https://myapp.com/                                  DNS
- Wildcards can also be used in the Domain Names (e.g., *.example.com)
- Must have a valid SSL/TLS Certificate for
  - Your domain name
    - All Alternate Domain Names you added to your                 SSL/TLS Certificate CloudFront distribution                                                             *.example.com www.example.com
- Certificate must be available in US-EAST-1 (N. Virginia) CloudFront region


## Exercise - HTTPS static website using S3 and CloudFront
*(Slide 783)*

Pre-requisites:
1. Should have your own domain name
2. Should have S3 bucket configured as a static website Route53 Public Hosted Zone                         Create TLS certificate for your domain name and www 1 subdomain using Amazon Certificate Manager (ACM) CloudFront distribution
in N. Virginia region yourdomain?
2   Create CloudFront distribution with S3 as an origin. S3 policy to allow OAC. Set viewer protocol policy to SSL Certificate            redirect HTTP to HTTPS. Set the alternate domain names (CNAME) for your www subdomain.
3   Update Route53 record for www to point to CloudFront distribution CloudFront               S3 4   Access website using your domain name. It should redirect to https://www.domainname and website should be accessible


## Exercise resources
*(Slide 784)*

- Download the sample website content from the   { resource section                                   "Version": "2012-10-17",
- Use this bucket policy to make the bucket public. Replace bucket name.                       "Statement": [ { "Sid": "ForStaticWebsite", "Effect": "Allow", "Principal": "*", "Action": [ "s3:GetObject" ], "Resource": [ "arn:aws:s3:::bucket-name/*" ] } ] }


## CloudFront Pricing (Until Nov 2025)
*(Slide 785)*

- CloudFront pricing is mainly based on Data Transfer Out (DTO) + Request count.
- Data transfer from AWS origins to CloudFront is free.                                                            $$
- Additional optional charges: Cache invalidations, Lambda@Edge, Logs etc.
- Price Classes let you reduce cost by limiting which edge locations CloudFront uses:
  - Price Class 100 = Cheapest
  - Price Class 200 = Balanced Free
  - Price Class All = Best performance.
- Performance may reduce if edge locations are restricted.
- Default is Price Class All unless you change it.
Price Class                        What it Means                               Usage
Price Class All                          Uses all edge locations worldwide       Best performance, highest cost
Price Class 200                          Uses most regions, excludes costliest   Balanced performance & cost
Uses only cheapest regions (US, Price Class 100                                                                  Lowest cost, lower global performance Canada, Europe)


## CloudFront Price Classes (Until Nov 2025)
*(Slide 786)*

```text
                                                   PC200
                                          PC200   PC100
                                         PC100

                                                                     PC200
                                                           PC200


                                                                   PC200


                                                     PC200
```


## CloudFront Pricing (From Nov 2025)
*(Slide 787)*

- New Pricing: Flat-rate pricing (fixed monthly CloudFront cost with no overages.)
- CloudFront flat-rate pricing bundles CDN usage and related AWS features into a single monthly fee with no overage charges.
- Flat-rate pricing plans include the following for a monthly price:
  - CloudFront CDN
  - AWS WAF and DDoS protection
  - Bot management and analytics
  - Amazon Route 53 DNS
  - Amazon CloudWatch Logs ingestion
  - TLS certificate
  - Serverless edge compute
  - Amazon S3 storage credits each month
- Start with the $0/month Free plan and upgrade to access more capabilities and larger usage allowances.


## AWS Global Accelerator
*(Slide 788)*


## AWS Global Accelerator
*(Slide 789)*

- Similar to Amazon CloudFront, AWS Global Accelerator also      Client               Client          Client uses AWS edge locations Anycast IP (BGP)
- Supports endpoints as Application Load Balancer, Network Load Balancer, EC2 instances and Elastic IPs Edge locations        Edge locations
- Allocates 2 Anycast IPs
- Anycast IPs send traffic directly to Edge Locations
- It’s possible to whitelist these IP addresses on client side
- Supports TCP/UDP protocols not just HTTP/S thereby suitable for VoIP, Gaming, Streaming, IoT applications              Region 1                   Region 2
- Performs health checks and automatic failover across Regions. Endpoint                  Endpoint
- Improves availability across multi-Region architectures.
- Built-in DDoS protection with AWS Shield                                  Nearest Region             Failover Region


## CloudFront vs Global Accelerator
*(Slide 790)*

```text
                                                            CloudFront                             Global Accelerator

                      Functionality           Content Delivery which optimizes        Global Accelerator optimizes network routing
                                              HTTP/HTTPS content delivery for both    for any TCP/UDP application, improving
                                              static and dynamic using Edge network   performance and availability by routing traffic
                                              and Edge Cache                          to nearest and health endpoints
                     How to access                             DNS                                2 Static (Anycast) IPs

                         Caching                  Yes (One of the primary feature)                          No

                        TCP/UDP                                 No                                          Yes

                   Failover Feature                           Limited                          Yes, performs health check

                        Use cases                 Website, Media streaming, APIs                VoIP, Gaming, IoT (MQTT)

                   DDoS Protection                  Yes, works with AWS Shield                 Yes, works with AWS Shield
```


# Route 53 (DNS)

*(Source: Slide 791)*

### Amazon Route 53

*A domain registration and DNS service*


## In this section..
*(Slide 792)*

- DNS Basics
- Amazon Route 53
- Route 53 Hosted Zones
- Route 53 DNS Records types
- Route 53 Health Checks
- Route 53 Records TTL
- Route 53 Routing policies
- Route 53 Resolver endpoints and Hybrid DNS + Hands-on exercises and demos


## DNS Basics
*(Slide 793)*


## What is DNS?
*(Slide 794)*

- Domain Name System which translates the human friendly hostnames into the machine IP addresses such as www.google.com => 142.250.205.46
- DNS is the backbone of the Internet DNS Server
11.22.33.44 Client www.example.com 11.22.33.44


## DNS Components and terminologies
*(Slide 795)*

- Domain Registrar - A company authorized to register and manage domain names on the internet. Ex. Amazon Route 53, Namecheap, GoDaddy, Domain.com etc.
- Top Level Domains (TLD) - .com, .us, .gov
- Zone Apex – Main domain e.g. amazon.com, example.com, awswithchetan.com
- Subdomain or Second Level Domains (SLD) – api.amazon.com, www.awswithchetan.com
- Fully Qualified Domain Names (FQDN) - FQDN uniquely identifies one specific host on the internet
- Name Servers – Answers DNS queries by looking up records stored in the zone file
https://webserver.awswithchetan.com. Protocol        Subdomain      SLD             TLD    Root Zone Apex FQDN


## DNS Components and terminologies
*(Slide 796)*

- Domain Registrar - A company authorized to register and manage domain names on the internet. Ex. Amazon Route 53, Namecheap, GoDaddy, Domain.com etc.
- Top Level Domains (TLD) - .com, .us, .gov
- Zone Apex – Main domain e.g. amazon.com, example.com, awswithchetan.com
- Subdomain or Second Level Domains (SLD) – api.amazon.com, www.awswithchetan.com
- Fully Qualified Domain Names (FQDN) - FQDN uniquely identifies one specific host on the internet
- Name Servers – Answers DNS queries by looking up records stored in the zone file
- Zone Files – Text file containing DNS records (A, AAAA, CNAME etc.)


## How DNS resolution works?
*(Slide 797)*

```text
                                                   example.com


                                      11.22.33.44
                                                                                       6 example.com?

                                                                                      7 example.com IP
                                                                                         (11.22.33.44)
                                                                            4 example.com?

                    9                                                       5 example.com NS


                                1 example.com?                        2 example.com?
      TTL
                                8 11.22.33.44                         3 .com NS
                                                        TTL
                                                 Local DNS Server                  Root DNS              .com DNS    example.com
                                                                                    Server                 Server     NS Server

                                              Assigned and Managed                 Managed by           Managed by     Managed by
                                              by your company or by                  ICANN                IANA       domain registrar
                                                     your ISP
```


## DNS Record types
*(Slide 798)*

- A – Maps a domain to an IPv4 address                     example.com → 11.22.33.44
- AAAA – Maps a domain to an IPv6 address                  example.com → 2001:db8:abcd:1234::1
- CNAME – Maps a domain to another domain name             www.example.com → example.com
- MX – Mail exchange servers for a domain                  example.com → mail.example.com (priority 10)
- TXT – Stores text data (SPF, DKIM, verification, etc.) example.com → "v=spf1 include:_spf.google.com ~all"
- NS – Delegates a DNS zone to name servers example.com → ns1.awsdns-12.com
- SOA – Start of authority for the hosted zone example.com → Primary NS: ns1.awsdns-12.com,
- SRV – Defines service location (protocol, port)          Admin: admin.example.com _chat._tcp.example.com → target chatserver.example.com, port 5222
- PTR – Reverse DNS (IP → domain)
- CAA – Defines which CAs can issue certificates           44.33.22.11.in-addr.arpa → example.com
- and more..                                               example.com → issue "letsencrypt.org"


## Amazon Route 53
*(Slide 799)*


## Amazon Route 53
*(Slide 800)*

- A fully managed global DNS service by AWS                               example.com?
- Route 53 is also a Domain Registrar 11.22.33.44 Client                     Amazon Route 53
- The only AWS service which provides 100% availability SLA
Public IP 11.22.33.44 Why 53 in                       53 is a traditional Route 53?                                                        EC2 Instance DNS port


## Route 53 Hosted Zones
*(Slide 801)*

- Public Hosted Zone – For public domain names. DNS resolution over the internet.
- Private Hosted Zone – For private domain names. DNS resolution within the VPC.
- Zone file contains records to point domain name to target IP address or to another domain name.
Public Hosted Zone                                       Private Hosted Zone Zone File Zone File app.myapp.com -> 10.0.0.5 www.example.com-> 11.22.33.44                            db.myapp.com -> 10.0.0.7 order.myapp.com -> 10.0.0.11 Public Hosted zone                                Private Hosted zone   …
Region                                            Region
app                    db Webserver
Public or Elastic IP                               Private IP            Private IP (11.22.33.44)                                    (10.0.0.5)            (10.0.0.7)


## Route53 DNS Records
*(Slide 802)*

- Use A record to point domain name to IPv4 address                                              A or AAAA record   EC2 Instance (IP address)
- Use AAAA record to point domain name to IPv6 address CNAME or Alias
- Use CNAME record to point a sub-domain to                                Application another domain name                                                     Load Balancer
- Use Alias record to point the apex domain or sub-    CNAME or Alias API Gateway domain to AWS provided DNS for AWS services e.g. ALB, CloudFront distribution, S3 website, API gateway etc. CNAME or Alias      S3 bucket (Website)
CNAME or Alias Amazon CloudFront


## CNAME vs ALIAS record
*(Slide 803)*

CNAME                                     ALIAS
What is it?            Points hostname to another hostname        Points hostname to AWS resource
Standard DNS record                             Yes                          No – AWS Route 53 specific
Supported at zone                              No                                       Yes apex (example.com) DNS Query charges                              Yes                                      No (Cost) Auto detect IP change                  No – follows normal TTL              Yes – Do not wait for TTL expiry
- Target is outside AWS (any DNS)
- Target is an AWS resource (CloudFront, ALB/NLB, S3 website, API gateway etc.)
- You need to create a record for zone-apex


## Exercise – Route 53 Private Hosted Zone
*(Slide 804)*

```text
                                                                                1   Launch 2 EC2 instances in default VPC.
                                                                                    Allow SSH and Ping (ICMP) from
                                                                                    anywhere in the security group

                                                   app.myapp.com -> 10.0.0.5    2   Create Route 53 Privat Hosted Zone with
                                                   db.myapp.com -> 10.0.0.7         some name say myapp.com

                             Private Hosted zone
                                                                                3   Create A records in hosted zone for app and
                                                                                    db and point to EC2 instances private IPs.
                                 Region


                                                app                  db         4   Login to first (app) EC2 instance using
                                                                                    EC2 instance connect and ping to
                                              Private IP           Private IP       db.myapp.com
                                              (10.0.0.5)           (10.0.0.7)
                                                                                5   DNS should resolve to db instance private IP
                                                                                    address and ping should be successful.

                                                                                6   Cleanup - Terminate both EC2 instances and
                                                                                    delete Route 53 Private hosted zone.
```


## Route 53 Record TTL (time to live)
*(Slide 805)*

- TTL is the number of seconds a DNS resolver (or client machine) will cache a DNS record before requesting a fresh copy from authoritative DNS/Name server
- Helps reduce repeated DNS lookups, improving performance and lowering Route 53 query costs.
  - High TTL (e.g., 24 hours / 86400 sec):
    - Reduces DNS traffic and DNS query cost.
    - Ideal for static or rarely changing resources (stable IPs, static websites).
    - Disadvantage: Changes to DNS records propagate slowly because cached values remain active until TTL expires. TTL = 1hr
  - Low TTL (e.g., 300 seconds):
    - Faster DNS updates and propagation.
    - Recommended when performing migrations, testing, or switching         Resolver with            Authoritative resources.                                                               Cache                 Name Server
    - Disadvantage: More DNS queries = slightly higher latency and cost.
  - Best Practice is to use low TTL during planned changes, then increase TTL after validation to optimize cost and performance.


## Route 53 - Health Checks
*(Slide 806)*

- Route 53 Health Checks monitor the availability and health of endpoints such as EC2 instances, ALBs, NLBs, CloudFront, or public IPs.​
- Enables automated DNS failover using Route 53 routing policies by        Client ensuring that traffic is routed only to healthy resources.
- Health checks can be performed using HTTP, HTTPS, or TCP                                          Health Check Health Check protocols.​
- Commonly used with Failover routing policy to enable DNS-based                Region A            Region B failover but also supported with other routing policies.
- Default interval: 30 seconds (Fast check: 10 seconds with extra cost).
- Default threshold: 3 consecutive failures/successes to mark unhealthy/healthy.
- Health check works for Publicly reachable endpoints; Private resources cannot be directly health-checked by Route 53.
- For Private resource health check, use CloudWatch alarm–based health checks instead.


## Route 53 - Routing policies
*(Slide 807)*

- Defines how Route 53 should resolve the DNS queries, specially when there are multiple DNS records of the same hostname with different target values
- Supported Routing policies:
  - Simple Routing – Sends all traffic to a single resource.​
  - Failover Routing – Switches traffic to a standby resource if the primary fails.​
  - Weighted Routing – Splits traffic across resources based on assigned weights.​
  - Latency Routing – Routes users to the region with the lowest network latency.​
  - Geolocation Routing – Routes users based on their geographic location.​
  - Geoproximity Routing – Routes based on resource location and allows traffic shifting between regions.​
  - IP-Based Routing – Routes traffic using the client’s source IP address.​
  - Multi-value Answer Routing – Returns up to 8 healthy records randomly for DNS queries.​


## Route 53 - Routing policies
*(Slide 808)*

Simple Routing Policy                                 Failover Routing policy Region A
Primary
Region B
Health Check
Secondary
- Typically to route traffic to a single resource
- To achieve Active-passive failover (generally across AWS regions)
- Optionally can specify multiple IPs and Route53
- Uses Health-checks returns all the IPs in the random order           •   You define Primary and Secondary (standby) records.
- Client to decide which IP to connect to.
- Primary is returned only if its health check is healthy.
- Health checks not supported, so all values are
- If the Primary becomes unhealthy, returns the Secondary. returned regardless of health.                    •   Secondary should point to a backup system (DR site, another region, static maintenance page, etc.).


## Demo – Failover Routing policy
*(Slide 809)*

```text
                                                             1   Launch 2 EC2 instances (Primary and Secondary)
                                                                 in different regions and configure them as a
                                                                 webserver. Allow HTTP from anywhere in SG.
                Client
                                                             2   Create Route 53 Public Hosted Zone for your
                                                                 domain name. (You should own the domain)
                                              Health Check
                             Health Check
                                                             3   Create health check for the Primary webserver in
                       Mumbai                 N. Virginia        Route 53

                                                             4   Create A record with Failover Routing policypointing to
                                                                 Primary Public IP. Create another A record pointing to
                                                                 Secondary Public IP with Failover Routing policy
                                                                 (Secondary)
                                                             5   Access using you domain name. Always points to
                                                                 Primary.
                                                             6   Stop Primary EC2 instance to simulate failure. Wait for
                                                                 health-check to fail.
                                                             7   Access domain name, should point to Secondary.
```


## Route 53 - Routing policies
*(Slide 810)*

Weighted Routing Policy
- You create multiple records with the same name and type with each record assigned a weight.
- Route 53 routes traffic proportionally based on those weights 10% (e.g., 70/30, 50/50, 90/10). 30%
- Useful for load balancing, testing new versions, and 60% gradual traffic shifting.
- Supports health checks - unhealthy records are removed from responses.
- You can set a weight of 0 to exclude that target from routing


## Route 53 - Routing policies
*(Slide 811)*

Latency Routing Policy Region A
- Routes users to the AWS region that provides the lowest network latency for them.
- You create records with the same name but in different AWS regions.                                                          Region B
- Uses latency measurements between AWS regions and the user’s DNS resolver.
- Supports health checks - Route 53 avoids unhealthy endpoints. Region C
- Ideal for latency sensitive global applications.


## Route 53 - Routing policies
*(Slide 812)*

Geolocation Routing Policy                 Geoproximity Routing policy
- Routes users based on their physical location
- Routes users based on geographic distance (country, continent, or specific state).            between the user and your AWS or on-prem endpoints.


## Route 53 – Routing policies
*(Slide 813)*

*Geolocation Routing Policy*


## Geoproximity Routing Policy
*(Slide 814)*

```text
                                              Europe (Frankfurt)
        US West (Oregon)

                                                                                               Asia (Tokyo)
                                                                       Middle east (Bahrain)




          Bias +25




                                                  Africa (Cape Town)
```


## Geoproximity Routing Policy
*(Slide 815)*

```text
                                                                                               Bias -25
                                              Europe (Frankfurt)
        US West (Oregon)

                                                                                                   Asia (Tokyo)
                                                                       Middle east (Bahrain)




          Bias +25




                                                  Africa (Cape Town)
```


## Geoproximity Routing Policy
*(Slide 816)*

```text
                                              Europe (Frankfurt)
        US West (Oregon)

                                                                                               Asia (Tokyo)
                                                                       Middle east (Bahrain)




                                                  Africa (Cape Town)
```


## Route 53 - Routing policies
*(Slide 817)*

Geolocation Routing Policy                   Geoproximity Routing policy
- Routes users based on their physical location
- Routes users based on geographic distance (country, continent, or specific state).              between the user and your AWS or on-prem
- You explicitly assign which location goes to          endpoints. which endpoint.                                   •   You can add a bias to shift more or less traffic
- Strict mapping - if a user is from India, they        toward a specific endpoint. will always go to the endpoint configured for     •   Set bias 1 to 99 to expand the size of the India.                                                geographic region
- Useful for compliance, content localization, or
- Set bias -1 to -99 to shrink the size of the region-restricted applications.                       geographic region
- Must have a default record for unmatched
- More flexible — traffic can shift dynamically as locations.                                            distances or biases change.
- Requires Route 53 Traffic Flow (policy-based routing).


## Route 53 - Routing policies
*(Slide 818)*

IP-based Routing policy 54.11.5.0/24    Region A
- Routes traffic based on the client’s IP address range (CIDR blocks).
- You create records mapped to specific IP ranges                                                   11.22.33.44
- Gives fine-grained control, more precise than Geolocation (country- level). Region B
- Useful when you want certain networks, ISPs, or corporate ranges to 12.23.34.45 reach specific endpoints.
Record Name                        Value             IP CIDR Region C example.com                    11.22.33.44       54.11.5.0/24 example.com                    12.23.34.45       70.20.9.0/24                                  13.24.35.46
example.com                    13.24.35.46       205.20.7.0/24 205.20.7.0/24


## Route 53 - Routing policies
*(Slide 819)*

Multi-value Routing policy Region A
- Allows multiple records with the same name and type
- Route 53 returns up to 8 healthy IPs in each DNS response.                           11.22.33.44
- The client chooses one of those IPs to connect to.
- Supports health checks - unhealthy endpoints are automatically removed. Region B
- Useful for distributing traffic across multiple servers or endpoints. 12.23.34.45
- Provides basic DNS-level load balancing (not as advanced as ELB).
Record Name                      A                                    Region C example.com                11.22.33.44 example.com                12.23.34.45                                         13.24.35.46
example.com                13.24.35.46


## Route 53 DNS Resolver
*(Slide 820)*

- VPC comes with default DNS server also                                   VPC                          10.10.0.0/16 called as Route53 DNS Resolver Subnet
- Runs at VPC Base + 2                                                                               Subnet
- Resolves DNS requests from:                                                         10.10.0.15               10.10.1.20
  - Route 53 Private Hosted Zone
  - Forwards other requests to Public DNS (including Route 53 Public Hosted Zones) 10.10.0.0/24             10.10.1.0/24
- Accessible from within the VPC Route 53 DNS queries
- Can not be access from outside VPC                 Private hosted zone
- Can not access other DNS (e.g. on-premises) 10.10.0.2 (VPC + 2)
Route53 DNS Public                            Resolver DNS


## Hybrid DNS
*(Slide 821)*

```text
                                                                                      VPC
                                                                                                      Private subnet   Private subnet


                                                  Public/Private Hosted Zone


                                                                                                         10.0.0.11     10.0.1.22



                    Corporate                                                  Route53 DNS Resolver
                    data center        DNS
                                     Forwarder


                                          DNS
             192.168.0.17                Server                 VPN/DX
```


## Route 53 – Inbound resolver endpoint
*(Slide 822)*

```text
                                                                                      VPC
                                                                                                      Private subnet   Private subnet


                                    Public DNS    Public/Private Hosted Zone


                                                                                                        10.0.0.11      10.0.1.22



                    Corporate                                                  Route53 DNS Resolver
                    data center        DNS
                                     Forwarder


                                          DNS                                                                            Inbound
                                                                                                         Inbound
             192.168.0.17                Server                 VPN/DX
```


## Route 53 – Outbound resolver endpoint
*(Slide 823)*

```text
                                                                                      VPC
                                                                                                      Private subnet   Private subnet


                                    Public DNS    Public/Private Hosted Zone


                                                                                                       10.0.0.11       10.0.1.22
                                                                                               Conditional
                                                                                               Forwarding

                    Corporate                                                  Route53 DNS Resolver
                    data center        DNS                                                              Outbound        Outbound
                                     Forwarder


                                          DNS
             192.168.0.17                Server                 VPN/DX
```


# AWS Serverless (API Gateway and Lambda)

*(Source: Slide 824)*

### AWS Serverless


## AWS Serverless Services
*(Slide 825)*

```text
              API and Application                                         Storage and Databases
                                                        Compute
                 Integrations




           API Gateway AWS AppSync                      AWS Lambda                  S3




            SQS             SNS           EventBridge   AWS Fargate   DynamoDB    Aurora     Redshift
                                                                                 Serverless Serverless
```


## Serverless
*(Slide 826)*

```text
        ✓No infrastructure management
        ✓Automatic scaling
        ✓Pay-per-use pricing
        ✓Built-in availability and fault tolerance
        ✓Event-driven execution
        ✓Short-lived, stateless compute
        ✓Faster development
```


## A simple weather app
*(Slide 827)*

```text
                 IoT sensors




                                              GET API (city=Pune)
                                                                       Web          Query
                                                                    Application               Database



          Browser or Mobile App
                                                                           A simple Weather App
```


## Hosting weather app on EC2
*(Slide 828)*

```text
                                              GET API (city=Pune)              Query



                                                                    EC2                      RDS


                                                                      A simple Weather App
```


## Hosting weather app on EC2
*(Slide 829)*

```text
         What if there is heavy traffic?




                                              GET API (city=Pune)              Query



                                                                    EC2                      RDS


                                                                      A simple Weather App
```


## Hosting application on EC2
*(Slide 830)*

```text
        What if there is heavy traffic?
                                                                    Vertical Scaling




                                              GET API (city=Pune)                      Query




                                                                                               RDS
                                                                         EC2

                                                                A simple Weather App

        ✓ Simple architecture                  X Infrastructure Management
                                               X High cost even when no traffic
```


## Hosting application on EC2
*(Slide 831)*

```text
        What if there is heavy traffic?
                                                                    Horizontal Scaling




                                              GET API (city=Pune)                        Query




                                                                                                 RDS
                                                                             EC2

                                                                A simple Weather App
```


## Hosting application on EC2
*(Slide 832)*

```text
        What if there is heavy traffic?
                                                                 Horizontal Scaling




                                         GET API (city=Pune)
                                                                                        Query



                                                               ELB
                                                                                                RDS
                                                                          EC2

                                                                 A simple Weather App

        ✓ Highly Available                      X Infrastructure Management
                                                X High cost even when no
                                                  traffic
```


## Hosting application on EC2
*(Slide 833)*

```text
        What if there is no traffic?
                                                                  Horizontal Scaling

                                                                              Fixed hourly cost

                                            Fixed hourly cost                                             Fixed hourly cost
                                         GET API (city=Pune)
                                                                                            Query

                                                                              Fixed hourly cost
                                                                ELB
                                                                                                    RDS
                                                                           EC2

                                                                  A simple Weather App

        ✓ Highly Available                      X Infrastructure Management
                                                X High cost even when no
                                                  traffic
```


## Hosting weather app using AWS Lambda
*(Slide 834)*

```text
                                               Fixed hourly cost                                    Fixed hourly cost
                                         GET API (city=Pune)           invoke             Query



                                                             ELB                Lambda
                                                                                                  RDS


                                                                   A simple Weather App

        ✓ Highly Available and Scalable compute
        ✓ No cost for Lambda when no traffic
```


## A serverless weather app
*(Slide 835)*

```text
                                         GET API (city=Pune)        invoke             Query


                               User
                                                          API Gateway        Lambda            DynamoDB


                                                               A simple Weather App – Fully Serverless

        ✓     Highly Available, Scalable
        ✓     No infrastructure management
        ✓     No infrastructure cost when no traffic
        ✓     Pay per unit
```


## Cost comparison                                                             Total monthly requests = 10k x 30 = 300k
*(Slide 836)*

```text
                                                                                           Service       Cost/hr          Monthly
            Option 1
                                                                                       EC2              $0.101          730 hr x
                                                                                       m5.large (2                      $0.101 x 2 =
                                                                                       vcpu, 8G                         $147.46
               GET API (city=Pune)
                                                                      Query            Memory) x 2
                                                                                       ELB              $0.0239         730 hr x
    Users                                                                                                               $0.0239 =
                                              ELB                             RDS                                       $17.44

                                                             EC2                       Total                            $164.9

                                                                                           Service       Cost/request       Monthly
                                                                                      Lambda            $0.0000166667      $1.25
                                                                                      (128MB)            for every GB-
                GET API (city=Pune)                 invoke                                              second
                                                                      Query
                                                                                      Lambda            $0.20 per 1M       $0.08
     Users                                                                            Requests          requests
                                         API Gateway         Lambda       DynamoDB
                                                                                      API Gateway       $3.50 per 1M       $1.20

           Option 2                                                                   Total                                $2.53
                                                                                      Assumption: 128mb memory, 2 sec execution time
```


## Amazon API Gateway
*(Slide 837)*

- API Gateway is a fully managed API service
- API gateway provides:
  - REST APIs, HTTPS APIs and WebSocket APIs
  - Use AWS Lambda, Amazon ECS, EKS, Elastic Beanstalk etc. for hosting backend services.                                      API Backend
  - Out of the box monitoring and logging with Amazon CloudWatch                                                     Client         API Gateway
  - Support User authentication & authorization, API throttling, API keys etc.
  - Response cache, API versioning, importing APIs, AWS WAF integration and many such features.


## API Gateway – REST and HTTP APIs
*(Slide 838)*

- REST APIs and HTTP APIs are both RESTful API products.
- HTTP APIs are designed with minimal features at a lower price.
- Choose REST APIs if you need features such as API keys, per-client throttling, request validation, AWS WAF integration, or private API endpoints.
Lambda Functions REST and HTTP APIs Public Endpoints on EC2
Other AWS Services       Mobile client      Web client services API Gateway request / response                            Publicly accessible endpoints


## API Gateway – WebSocket APIs
*(Slide 839)*

- Uses persistent WebSocket connections instead of request/response like REST.
- Supports real-time, bidirectional communication between clients and the server.
- Ideal for chat apps, notifications, live dashboards, multiplayer games, IoT updates, etc.
Lambda Functions WebSocket APIs Public Endpoints on EC2
Other AWS Services       Mobile client      Web client Websocket                                       services API Gateway Publicly accessible endpoints


## API gateway method integrations
*(Slide 840)*

An integration in API Gateway defines how an API method connects to and invokes its backend
- Lambda Integration – Triggers AWS Lambda functions; perfect for fully serverless backend logic.
- HTTP / HTTP Proxy Integration – Sends requests to public HTTP endpoints or proxies the entire request to your backend as-is.
- AWS Service Integration – Directly invokes AWS services without writing backend code. This supports AWS services including SQS, SNS, Step Functions, DynamoDB, Kinesis, EventBridge and many many more.
- Mock Integration – Returns a predefined response without calling any backend; great for demos and testing.
- VPC Link Integration – Privately connects to services inside your VPC via an ALB/NLB, without exposing them publicly.


## API gateway method integrations
*(Slide 841)*

SQS Commonly used AWS services integration
- SQS – Send messages SNS
- SNS – Publish notifications
- Step Functions – Start workflows
- Kinesis – Put records                                  EventBridge
- DynamoDB – CRUD operations API Gateway
- EventBridge – Put events Step Functions
Kinesis
DynamoDB


## API Gateway Endpoints
*(Slide 842)*

Edge-Optimized Endpoint (for REST APIs)
- Uses CloudFront to route requests through AWS edge locations. $$
- Best for global clients needing low latency. API Gateway Regional Endpoint CloudFront (PoPs)
- API is deployed in a specific AWS Region.
- Best when clients are mainly in the same region or when using your own custom CDN or private networking. $ Private Endpoint API Gateway
- Accessible only from your VPC using an Interface VPC Endpoint (PrivateLink).
- Not publicly reachable — ideal for secure, internal, service-to- service communication. $ VPC endpoint API Gateway


## API Gateway - Custom domain name
*(Slide 843)*

- By default, API endpoint receives AWS provided domain name                                                        Route 53                   ACM https://{api-id}.execute-api.{region}.amazonaws.com/{stage}
- We can use custom domain name e.g. api.myapp.com
- You must provide an SSL/TLS certificate from ACM                   api.myapp.com (DNS)
- For edge-optimized REST APIs, the domain is deployed through CloudFront automatically and hence Certificate                    HTTPS/TLS must be in us-east-1 region                                                                      Backend
- For Regional REST API or HTTP API, Certificate must be in   Mobile/ Web API gateway client the same region as the API


## API gateway integration with private backend services
*(Slide 844)*

- API gateway is a Public service and sits         AWS Cloud outside of the VPC
- It accesses backend service endpoints                                      VPC using their Public DNS https://alb-public-dns   ECS
- It can not directly access the private resources inside a VPC e.g. EC2 instance Private IP, internal application                             ALB load balancer etc.                          API gateway             (public)


## API gateway integration with private backend services
*(Slide 845)*

- To reach private resources (like EC2, ECS, internal ALBs) you can use API Gateway VPC link V2.
- Creates a private connection between API Gateway and ALB hosted inside your VPC
- The API backend targets can be EC2 instances, ECS tasks/services or On-premises servers which are accessible through the ALB (hybrid-connectivity)
VPC
Mobile/ Web client                            API gateway   VPC Link V2   ALB       EC2 Instances Direct Connect Note: VPC link V1 is legacy which supports NLB


## API Authentication & Authorization
*(Slide 846)*

- Using IAM Permissions
- Using Amazon Cognito
- Using Lambda Authorizer (custom authorizer)
- More ways..


## Using IAM
*(Slide 847)*

{                                Caller sample IAM policy "Version":"2012-10-17",
- Useful when APIs are internal or API to be invoked "Statement": [ by other AWS services e.g. Lambda, Step { Functions, ECS task etc.                                               "Effect": "Allow", "Action": [
- API Request must include method’s                                         "execute-api:Invoke" authorizationType=AWS_IAM                                              ], "Resource": [ "arn:aws:execute-api:us-east-1:account-id:api- id/*/GET/pets" ] }, { "Effect": "Deny", "Action": [ IAM "execute-api:Invoke" ], 403 Forbidden 2              "Resource": [ 3             4             "arn:aws:execute-api:us-east-1:account-id:api- id/*/POST/pets" 1 SigV4 signed                                    ] } AWS Service                                API Gateway          ] }


## Using Amazon Cognito
*(Slide 848)*

Amazon Cognito is an AWS service that provides secure user sign-up, sign-in, and access control for web and mobile apps, acting as a managed identity provider (IdP)
Authentication and Authorization for API gateway:
1. User signs-in to Amazon Cognito User pool and receives JWT tokens 3
2. Sends JWT tokens to API gateway endpoint                  1            Amazon Cognito authenticate                           Validate
3. API gateway validates JWT tokens
4. If invalid -> 401 Un-athorized                                           401 Unauthorized 4              5
5. If invalid -> Retrieve claims (email, groups, custom                  2 Tokens (JWT) claims) and forwards the request to the backend            User                             API Gateway
6. Backend application handles the request and responds back


## Using Lambda Authorizer
*(Slide 849)*

Lambda authorization should be used when you want full flexibility to implement custom authentication and fine-grained authorization tailored to your application's needs.
Authentication and Authorization for API gateway:                                                          4 Validate
1. User signs in with a 3rd-party IdP and the client obtains tokens (JWT/opaque token/keys). 3rd party provider       Lambda Authorizer
2. Client calls the API Gateway endpoint and includes the token in the request header.                                          1                                        3     5        IAM authenticate
3. API Gateway invokes the configured Lambda authorizer                                                                   policy
4. Lambda authorizer validates the token                                          2 Tokens                                  6
5. If valid, Lambda returns an IAM-style policy Document, User principal Id, and optional context to API Gateway.
6. API Gateway evaluates the policy; if allowed it forwards the request to the backend


## API Gateway Resource policy
*(Slide 850)*

- Resource Policies control who can access your API at the network and account level, before authentication/authorization happens.                        { "Version": "2012-10-17",
- They work similarly to S3 bucket policies - attached           "Statement": [ directly to the API Gateway resource.                            { "Effect": "Allow",
- Useful for restricting public APIs to certain IPs or AWS "Principal": "*", accounts even before other auth layers. "Action": "execute-api:Invoke",
- Let you allow or deny access based on:                             "Resource": "arn:aws:execute-api:us-
  - AWS accounts                                             east-1:111111111111:api-id/*", "Condition": {
  - IAM principals "IpAddress": {
  - Source IP ranges                                                   "aws:SourceIp": "203.0.113.0/24"
  - VPC endpoints (for Private APIs)                                 }
  - AWS Organizations                                              } }
- Critical for Private APIs, where access must be restricted     ] to specific VPC Interface Endpoints.                         }


## Amazon API Gateway – Good to know
*(Slide 851)*

- API Versioning – Lets you publish multiple versions of your API (e.g., v1, v2) so clients can upgrade safely without breaking existing integrations.
- API Stages & Environments – Provides separate deployments like dev, test, and prod, each with independent settings, logging, throttling, and stage variables.
- API Keys – Used to identify and throttle clients through usage plans; not meant for authentication or security by themselves.
- API Caching – Improves performance by caching API responses at the stage level, reducing backend load and lowering latency.
- Request & Response Transformation - Enables mapping templates to modify headers, payloads, or formats between client and backend (e.g., JSON <> XML).
- AWS WAF Integration – You can attach AWS WAF to API Gateway (regional REST & HTTP APIs) to protect against common web exploits like SQL injection, XSS, bot attacks, and abnormal request patterns.


## Amazon API Gateway – Summary
*(Slide 852)*

- API Gateway supports REST, HTTP, and WebSocket APIs, enabling traditional request/response, lightweight microservices, and real-time bidirectional communication.
- APIs method integrations – Lambda, HTTP/proxy, AWS services, Mock and VPC Link.
- API Gateway offers edge-optimized, regional, and private endpoints to match global performance, regional access, or VPC-only connectivity needs.
- API Gateway enforces HTTPS for all endpoints, and edge-optimized custom domains require the ACM TLS certificate to be created in the us-east-1 (N. Virginia) region.
- VPC Link v2 enables secure, high-performance private connectivity from API Gateway to services inside your VPC using ALBs without exposing them publicly. VPC Link V1 supports NLB.
- API Gateway supports IAM, Cognito User Pools, and Lambda Authorizers, allowing granular security through signatures, JWT tokens, or custom auth logic.
- You can map your APIs to custom domain names with ACM certificates.
- API Gateway provides versioning, stages/environments, API keys with usage plans, caching, request/response transformations, and WAF integration to improve manageability, performance, and security.


## AWS Lambda
*(Slide 853)*


## AWS Lambda
*(Slide 854)*

- A serverless, event-driven compute service that lets you run functions without provisioning servers (Function-as-a-Service).
- Ideal for stateless execution                                                      API
- Scales automatically as per #requests, messages etc.                EventBridge trigger
- Just write a function or upload a package with code and dependencies.                                                                      Event Lambda Function
- Supports many languages (Python, Go, Node.js, Java, C#, Powershell, Ruby, Go and more)                                           Kinesis SQS
- Pay as per number of invocations, execution time and assigned memory (Per GB-second).                                                        50+ event sources
- Memory: 128mb to 10gb
- Lambda integrates natively with CloudWatch for logs, metrics, and
- Max execution time: Up to 15 mins performance monitoring.                                              •   IAM role
- More..
- Uses IAM role to access other AWS services e.g. S3, DynamoDB New ! Lambda Durable functions


## Lambda function Versions and Aliases
*(Slide 855)*

- Versions are immutable snapshots of your Lambda code + configuration (timeout, memory, env vars). Version 1
- $LATEST represents the most recent deployed code.
- Aliases are named pointers (e.g., dev, test, prod) that map to                                   Code/Config change and Publish a specific version.
- Aliases allow safe deployments by switching traffic from one                 Alias = Version V2 Prod version to another without any change at client side.
- Aliases can use traffic shifting (weighted routing), example 80% to v3, 20% to v4 for canary releases.                                                  80% Alias =
- IAM policies, triggers (API Gateway, EventBridge), and                                                  Version V3 Dev permissions can be attached to aliases, enabling environment isolation.                                                                             20% $LATEST
Tip: Never use $LATEST in production. Use specific versions + aliases for reliability,                Version V4 rollback, and controlled releases.


## Exercise – A simple weather app with AWS Lambda
*(Slide 856)*

```text
                                                                                   1   Create DynamoDB table and add
                                                                                       sample items for cities and temperature
                                                                                       e.g. Pune (String), 28 (Number)

                                                                                   2   Create Lambda function (Python 3.11) and
                                                                                       add code provided with this lecture. Update
                                                                                       dynamodb table name.
    GET API (city=Pune)
                                                                                   3   Create IAM role for Lambda to have Read-
                                                                                       only permissions for DynamoDB. Configure
                                              invoke            Query
                                                                                       Lambda function to use this role and change
                                                                                       execution time limit to 1 min.

                            API Gateway                Lambda           DynamoDB   4   Create API Gateway REST API with Any
                                                                                       method and invoke Lambda function with
                                                                                       proxy integration. Deploy API (use any stage
                                                                                       name)
                                  A Weather App – Fully Serverless
                                                                                   5   Create sample HTML web page using the
                                                                                       code provided and replace API endpoint
                                                                                       with your endpoint.
                  Reference code files are provided with this lecture.             6   Open HTML page and access the simple
                                                                                       weather app.
```


## Lambda – Important to know
*(Slide 857)*

- Lambda Concurrency and Throttling
- Lambda Reserved Concurrency
- Lambda Cold start and Provisioned Concurrency
- Lambda Synchronous and Asynchronous execution
- Lambda SnapStart
- Lambda inside VPC


## Lambda Concurrency
*(Slide 858)*

- Lambda invokes your function in a secure and isolated execution environment.
- To handle a request, Lambda must first initialize an execution environment (the Init phase), before using it to invoke your function (the Invoke phase)
- By default, your account has a concurrency limit of 1,000 concurrent executions across all functions in a Region. 1                             2 Init      Execution 1           Execution 2
Example: For 10 requests your lambda function concurrency may look like this
Total concurrency = 6


## Lambda Concurrency
*(Slide 859)*

```text
        Formula to calculate desired concurrency that you need:

          Concurrency = (average requests per second) * (average request duration in seconds)


         Example:
         If average number of requests per second is 100 and average duration for lambda function to
         execute each request is 0.5 seconds, then desired concurrency = 100 x 0.5 = 50
```


## Reserved Concurrency
*(Slide 860)*

- By default, your account has a concurrency limit of 1,000 concurrent executions across all functions in a Region.
- Your functions share this pool of 1,000 concurrency on an on-demand basis.
- Your functions experiences throttling (they start to drop requests) if you run out of available concurrency.
- For critical functions in your account, you can set the minimum and maximum concurrency for the function by setting Reserved Concurrency
- When a function has reserved concurrency, no other function can use that concurrency.
- There is no cost for setting Reserved concurrency
Image source: https://docs.aws.amazon.com/lambda/latest/dg/lambda- concurrency.html


## Cold start and Provisioned Concurrency
*(Slide 861)*

- Before execution starts, Lambda has to download the code and initialize the environment
- This contributes to Cold start time for the function
- For latency sensitive executions, this might add                                   Lambda Cold Start considerable delay, and this cold start can be mitigated with Lambda Provisioned Concurrency.
- Provisioned Concurrency keeps the pre-initialized execution environments for the lambda function
- You pay for the Provisioned Concurrency even if there are no lambda functions currently consuming the capacity
- You can enable/disable Provisioned concurrency depending on expected load Example: Provisioned Concurrency of 6
Remember: Reserved Concurrency solves throttling issue, and Provisioned Concurrency solves Cold start issue


## Lambda Synchronous and Asynchronous invocation
*(Slide 862)*

Synchronous invocation
- Caller waits for the function to finish and receives the response immediately.
- On Lambda throttling, caller receives TooManyRequestsException (429) error
- API gateway, ALB makes synchronous lambda invocation                     Caller
- Use cases: APIs, Real-time processing (auth, validation, lookups)
If throttled (429) or system Asynchronous invocation                                                                              error (5xx) – Retry up to 6 hrs
  - Lambda queues the event internally and execution happens later.
  - On error, lambda retries 2 more times and then send to DLQ (SQS, SNS). You can also set failure targets.
  - If error due to throttling (429) or system errors (5xx) then sends       Caller event back to the queue and retries for up to 6 hrs in the interval from 1 sec to 5 mins                                                                                    after all retries
  - S3 Object events, SNS/EventBridge, DynamoDB streams, Kinesis          Dead-Letter-Queue (DLQ) streams etc. makes asynchronous lambda invocation                                                         On-failure
  - Use cases: Event processing, data processing, order processing


## Lambda SnapStart
*(Slide 863)*

- 10x faster function startup times
- No extra cost
- Minimal or no code changes to your application code. Initialize      Create Execution     Download    Start
- Supported for Java 11+, Python 3.12+ and .Net 8+ runtimes           environment      Code     Runtime Function       MircoVM code         Snapshot
- Takes Firecracker microVM snapshot of the memory and disk state of the initialized execution environment, encrypts the snapshot, and intelligently caches it. Restore
- For every invocation, restores this snapshot in milliseconds,                                  MircoVM Run Lambda Handler function avoiding repeated class loading, and dependency initialization.                                Snapshot
- Avoid generating unique data during init (e.g., random values,            Invoke                Restore UUIDs, credentials)                                                                            MircoVM Run Lambda Handler function Snapshot
- SnapStart is cheaper and faster than using Provisioned Concurrency however there are limitations like supported Restore runtime and initialization code needs to be stateless (e.g. no db                              MircoVM Run Lambda Handler function connections, global variables etc.)                                                            Snapshot


## Lambda in VPC
*(Slide 864)*

- Lambda function (by default) is provisioned outside of     Region VPC
- It can not access Private resources inside a VPC e.g. Databases, EC2 Private IPs, Internal ALB etc.
- You can create Lambda function inside VPC
- This creates an ENI in the VPC subnet through which it       VPC can access other resources inside VPC
- All other VPC controls apply to ENI e.g. Security Group,        Private Subnet   Private Subnet subnet route table etc.
- To allow Lambda to access internet, you must provision       Security group NAT gateway and configure route for Lambda subnet
- Use case: Lambda need to access RDS database, ElastiCache or OpenSearch service inside VPC                        ENI            RDS


## Lambda patterns and Use Cases
*(Slide 865)*


## Lambda common patterns
*(Slide 866)*

1. Execute on each API request - Backend for API gateway or Elastic load balancer API Gateway
2. Message processing – Read SQS messages as they arrive in the queue                                                             SNS     SQS
3. S3 event processing – Trigger lambda when Object is uploaded to S3 bucket                                                                 S3
4. Real-time data processing – Read data from Kinesis data stream and process in real-time                                                Kinesis
5. Event processing – EventBridge rule or schedule, CloudWatch alarm etc. EventB CloudWatch ridge


## Lambda common Use Cases
*(Slide 867)*

- File Processing Apps
  - PDF encryption
  - Image analysis
- Database Integration                                                               Rekognition
  - Asynchronous writes to Database
  - Database Event handler
SQS          Write to DB
Index
DynamoDB   Stream   Update indexes       OpenSearch


## Lambda common Use Cases
*(Slide 868)*

- Scheduled Tasks
  - Creating and sending reports send report
  - Trigger a maintenance activity EventBridge         Create Report                        SNS
- Incidence response
  - Trigger action based on CloudWatch Alarms
Systems            EC2
- Real-time stream processing                      CloudWatch Manager
  - Click-stream Analytics
  - IoT Sensor data processing Analytics
Kinesis Data Stream


## Let’s talk architecture
*(Slide 869)*


## Serverless voting app
*(Slide 870)*


## Architecture
*(Slide 871)*

```text
            Real-time (synchronous)

                                Authentication                    API Layer                  Backend        Data Layer
                                                                                                GetVote




                                                  Cognito                                      UpdateVote




                                                                               API gateway
                                                              /api/*                           DeleteVote            DynamoDB


             Mobile/ Web                                      *
               client                            CloudFront
                                                                  Static Website




                                                                                   S3
```


## Architecture
*(Slide 872)*

```text
        Event based (Asynchronous)

                                Authentication                API Layer                       Backend     Data Layer
                                                                                             GetVote
                                                                                    Buffer    Async


                                                  Cognito
                                                                                             UpdateVote
                                                                                               Async



                                                              /api/*                         DeleteVote            DynamoDB
                                                                                               Async


             Mobile/ Web                                        *
               client                            CloudFront
                                                              Static Website




                                                                               S3
```


## Watch video..
*(Slide 873)*

*Now, Kiro CLI*

*https://youtu.be/oy_yHDCuabI*


# AWS Integration Services

*(Source: Slide 874)*

*Amazon SQS, SNS and EventBridge*


## Why do we need Integration services?
*(Slide 875)*

- Modern applications rely on many services working together and how these services communicate determines how reliable and scalable the system is.
Order                            Database Service
Billing & Payment Backend UIService                     Service CRM
Product Catalog Search Email


## Why do we need Integration services?
*(Slide 876)*

- Modern applications rely on many services working together and how these services communicate determines how reliable and scalable the system is.
Solution: Loosely coupled systems
  - Enable asynchronous communication
  - Allow individual services to scale independently
  - Absorb sudden traffic spikes
  - Prevent failures in one service from impacting others


## Synchronous vs Asynchronous communication
*(Slide 877)*

*Synchronous communication   Asynchronous communication*


## A voting app..
*(Slide 878)*

```text
                                              API Server          Backend                        Database


                                                           ✓ Validate message
                                                           ✓ Validate fields
                                                           ✓ Add/process metadata
                                                           ✓ Build write query
                                                           ✓ Execute query and wait for the DB
                                                             write to succeed
                                                           ✓ Handle the failure
                                                           ✓ Respond back the success/failure
```


## A voting app..
*(Slide 879)*

```text
                                                                                   Backend


                                                           Lightweight
                                              API Server   processing
                                                                                   Backend   Database
                                                                         Message
                                                                          Queue
                                                                                   Backend
```


## Application Integration services
*(Slide 880)*

```text
                          Simple Queue Service (SQS)
                          Managed message queue service



                           Simple Notification Service (SNS)
                           Notification service which delivers SMS, Email,
                           Mobile Push notifications


                            Amazon EventBridge
                            Event bus enabling integration between AWS
                            services, SaaS and your applications
```


## Amazon SQS - Simple Queue Service
*(Slide 881)*

- Amazon SQS is a Highly available distributed message queue system.                             Producer Producer
- SQS supports two type of queues: Standard and FIFO Producer
- SQS supports multiple producers (writers) and consumers (readers) for the same queue.                                                                                  Send
How it works?
  - Producers send message to the queue.
  - Message is stored in the queue for the duration defined by retention period.                 SQS Receive
  - Consumers request message from the queue.
  - Request for message locks the messages during the processing by the consumer application (Visibility timeout).                                     Consumer Consumer
  - After processing consumer deletes the message (DeleteMessage api).                   Consumer
  - If not deleted, messages will stay in the queue until the retention period.


## Standard Queue vs FIFO Queue
*(Slide 882)*

```text
                                                          Standard Queue                                   FIFO Queue


                      Structure


                     Throughput               Nearly unlimited API requests per second.      Up to ~300 TPS without batching (up to
                                                                                                     ~3,000 with batching).
                       Delivery                 At-least-once — messages might be           Exactly-once processing — no duplicates.
                                                     delivered more than once.
               Message Ordering                 Best-effort ordering (not guaranteed).     Strict FIFO (first-in, first-out) within groups.
                          Cost                                 Lower                                           Higher
                                              For high throughput where occasional        Critical workflows needing exact order and
                   When to use?               duplicates or slight reordering is          deduplication.
                                              acceptable.
```


## Amazon SQS - Features
*(Slide 883)*

- Message Visibility Timeout
- Message Retention and Size
- Batch Operations
- Long polling
- Delay Queue
- Dead Letter Queue (DLQ)


## Amazon SQS – Features
*(Slide 884)*

Message Visibility Timeout Message requested          Message visible again (if
- When a consumer requests a message from SQS, a message                   (by one of the consumer)           not deleted) becomes temporarily invisible to other consumers Message
- Default visibility timeout = 30 seconds, can be set 0 sec-12 hrs.      visible
- Ideally message should be deleted after processed by the Message Not visible consumer.
- If message is not processed within Visibility timeout period, it becomes visible again and can be picked up by other consumers.
- A consumer can extend the visibility timeout time by calling the                       Visibility Timeout Period ChangeMessageVisibility API.
- If the visibility timeout is set too high and the consumer crashes,                                                            Time message re-processing will be delayed.
- If the visibility timeout is set too low, messages may be delivered              Message visible in the queue multiple times, increasing the chance of duplicates. Message not visible in the queue


## Amazon SQS – Features
*(Slide 885)*

Message Retention min       default                            max
- Messages can be retained in SQS queue for 1 min to 14 days (default 4 days)                                                Message Retention
Message Size                                                    1 min       4 days                         14 days
- Up to 1024 KB 1024 KB Batch Operations
- Amazon SQS batch actions allow you to send, receive, or delete up to 10 messages in a single API call.
- Reduces SQS API costs and improves application performance. Up to 10 messages in single API call
- Must use AWS APIs or SDK to use batch operations (SendMessageBatch,DeleteMessageBatch API)


## Amazon SQS – Features
*(Slide 886)*

Lambda with SQS
- Lambda uses Event source mapping to poll the messages from the SQS queue.
- No need to write polling code/logic. ~10000 msg/sec
- Event source mapping defines how many messages Lambda receives per invocation (batch size 1–10).
- Lambda scales concurrency based on queue depth
- Lambda deletes messages from the queue only after successful processing
- Supports partial batch response, allowing only failed messages to be retried.
- Works with both Standard and FIFO queues.
DLQ


## Amazon SQS – Features
*(Slide 887)*

Long Polling
- SQS waits up to 20 seconds for a message instead of        Consumer returning immediately with “no messages”.
- Reduces the number of SQS API calls, thereby lowers API cost
- Reduces application latency by returning the message as soon as it’s available in the queue.
- Can be configured at Standard or FIFO queue level or API level (using WaitTimeSeconds parameter)
Producer


## Amazon SQS – Features
*(Slide 888)*

Delay queues
- A delay queue postpones message delivery for a specified time.        SendMessage                      Message available
- Delay can be set from 0 to 15 minutes.                                     Message Not available
- Producers send messages normally, but consumers cannot see them until the delay expires.                                                   DelaySeconds time
- Delay can be applied per queue or per message (using DelaySeconds parameter).                                                                                                      Time
- Use case: Undo option after you click the Send button while sending an email Dead Letter Queue (DLQ)
- A DLQ stores messages that repeatedly fail to be processed.              Producer                                Consumer
- You configure a Maximum Receives (e.g., 5 attempts).
- After exceeding this count, the message is moved to the DLQ.                                       Failed to process
- Prevents “poison messages” from retrying forever.
- Useful to troubleshoot problematic payloads. DLQ


## Amazon SQS – Security
*(Slide 889)*

Encryption in Transit KMS
- All Amazon SQS API calls are secured using HTTPS, protecting data while it moves between producers, SQS, and consumers. Encryption at Rest
- SSE-SQS: Uses an AWS-managed SQS key with automatic management and rotation.
- SSE-KMS: Uses a customer-managed AWS KMS key for more control, auditing, and custom key policies.                                HTTPS         HTTPS Producer                          Consumer SQS Queue IAM Policy
- Controls who can access the queue and what actions they can perform (SendMessage,ReceiveMessage,DeleteMessage etc.).
- Used to allow cross-account access as well as access by other AWS services e.g. Simple Notification Service (SNS)


## Sample queue policies
*(Slide 890)*

```text
                             Allow cross-account access              Allow SNS to publish message
      {                                                   {
        "Version": "2012-10-17",                            "Version": "2012-10-17",
        "Id": "CrossAccountRoleAccess",                     "Id": "CrossAccountSNSPublish",
        "Statement": [                                      "Statement": [
          {                                                   {
            "Sid": "AllowRoleFromAnotherAccount",               "Sid": "AllowCrossAccountSNSToPublish",
            "Effect": "Allow",                                  "Effect": "Allow",
            "Principal": {                                      "Principal": "*",
               "AWS":                                           "Action": "sqs:SendMessage",
      "arn:aws:iam::222222222222:role/ExternalAppRole"          "Resource": "arn:aws:sqs:us-east-
            },                                            1:111111111111:MyQueue",
            "Action": [                                         "Condition": {
               "sqs:SendMessage",                                 "ArnEquals": {
               "sqs:ReceiveMessage",                                "aws:SourceArn": "arn:aws:sns:us-east-
               "sqs:DeleteMessage"                        1:111111111111:MySNSTopic"
             ],                                                   }
            "Resource": "arn:aws:sqs:us-east-                   }
      1:111111111111:MyQueue"                                 }
          }                                                 ]
        ]                                                 }
      }
```


## Exercise – Send message to SQS queue
*(Slide 891)*

```text
                                                                 1   Create SQS queue (standard queue)


                                                                 2   Use AWS CLI to Send message to the queue


                                                                 3   From SQS console, verify if you see the
                                send message                         messages


                                               Amazon SQS
       AWS CLI                                   Queue




           $aws sqs send-message --queue-url <QUEUE_URL> --message-body “Hello, this is test message 1”
```


## SQS architecture patterns
*(Slide 892)*

- Automatic scaling of Lambda functions based on SQS queue depth (# messages)
Producer
Producer
SQS Queue Producer
Lambda


## SQS architecture patterns
*(Slide 893)*

- Scaling EC2 instances based on SQS Queue depth (#messages)
Auto Scaling group
Producer
EC2 Instance Producer Scale Up/Down EC2 Instance Producer
Alarm
EC2 Instance CloudWatch


## SQS architecture patterns
*(Slide 894)*

- SQS as a buffer to process spiky requests received through API Gateway
SQS Queue API Gateway
Lambda


## Amazon SNS - Simple Notification Service
*(Slide 895)*

- Amazon SNS is a Pub/Sub notification service AWS SQS
- SNS enables application-to-application (A2A) and Application-to-Person (A2P) messaging.
- Decouples publishers and subscribers.                                             topic    AWS Lambda
- Ideal for microservices, distributed, and serverless apps.     Msg                              HTTP Publisher                          APIs
- Supports dead-letter queue (DLQ) for troubleshooting or reprocessing of the failed deliveries                                          Amazon     A2A Subscribers SNS
- SNS supports Standard Topic and FIFO Topic Failed deliveries Mobile Text
Email DLQ A2P Subscribers


## Amazon SNS – Topic types
*(Slide 896)*

```text
            You can create Standard Topic or FIFO Topic (like SQS)


                                                          Standard Topic                                FIFO Topic

                Message ordering                 Best efforts basis (not guaranteed)              Strictly first-in-first-out
                     Throughput                           Nearly unlimited                   ~300 messages/sec or 10MB/sec

                       Delivery                  At-least-once (messages might be             Exactly-once (No duplicates).
                                                     delivered more than once.)
                    Supported                 SQS, Kinesis Data Firehose, Lambda,                 Only FIFO SQS queue
                   subscriptions              HTTPS, SMS, Email, Mobile push
                          Cost                                 Lower                                       Higher

                                              Fan-out where ordering and duplicates    Critical workflows needing exact order and
                   When to use?               doesn’t create a problem.                deduplication.
```


## Amazon SNS – Message filtering
*(Slide 897)*

- By default, an Amazon SNS topic subscriber receives every message that's published to the topic.
- To receive only a subset of the messages, a subscriber must assign a filter policy to the topic subscription.
Message Filter policy {                                                                          { "Message": "New order created",               Amazon SNS                     "eventType": ["OrderCreated"], "MessageAttributes": {                                                       “source": [“mobile"] "eventType": {                                                         } "DataType": "String", "StringValue": "OrderCreated"                                                                              AWS SQS },                                             Topic                                  Filter policy "orderId": {                                                               { "DataType": "String",                                subscriptions       "eventType": ["OrderCreated"], "StringValue": "ORD-98765"                                               “source": [“web"] },                                                                     } "timestamp": {                                                                                              AWS Lambda "DataType": "String", Filter policy "StringValue": "2025-01-26T10:45:00Z"                                { },                                                                      "eventType": ["OrderCancelled"] HTTP "source": {                                                            }                                      APIs "DataType": "String", "StringValue": "web" } } }


## Exercise – Send email/SMS using SNS
*(Slide 898)*

```text
                                                      1   Create a SNS topic


                                                      2   Add email subscription for the topic. You will
                                              Topic
                                                          receive confirmation email. Click the link in the
                                                          email to confirm.
                         Publish message                  Add SMS subscription by adding your phone
                                                      3
                                                          number. You will receive text message with
                                                          code. Enter code to confirm your subscription.

                                                      4   Using SNS console, publish a message onto
                                                          the topic

                                                      5   Verify if you receive the message over an
                                                          email

                                                      Note: In some countries there are restrictions on sending
                                                      SMS like SMS are delivered only during specific time of
                                                      the day or promotional messages are blocked etc.
```


## Common architecture patterns
*(Slide 899)*


## SNS FIFO Topic
*(Slide 900)*

Booking/Reservation system
- Ordering is preserved
- No Duplicates
Customer booking FIFO Topic    FIFO Queue     Reservation   Database (Reservation)     Service


## SNS Fan-out
*(Slide 901)*

Fan-out means "Send once, deliver to many" for use cases where same message needs to be processed by multiple systems.
- Avoid sending the same message to many subscribers manually thereby simplifying application logic/errors/retries
- Prevent message loss if the producer crashes mid-delivery.                    Topic
- Easy to add new consumers later without changing the            Publisher
producer.


## SNS Fan-out patterns
*(Slide 902)*

```text
                                                                                                          SNS + SQS
        SNS and SQS queues are often used for Fan-out pattern when same message needs to be processed by
        different backend systems.




                                                             Payment Queue




                                                                                       Backend services
                                   Publish      Topic
            Order                  message                    Billing Queue
           Service
                                              Order placed


                                                             Shipping Queue




                                                             Inventory Queue
```


## SNS Fan-out patterns
*(Slide 903)*

```text
                                                                                  SNS + Firehose
        Fan-out to storage and analytics services (subscribe up to 5 Firehose)
                                                        Amazon Data
                                                          Firehose




                                                                                              Storage and Analytics Systems
                                                                                 S3




                                   Publish    Topic                              Redshift
           Event                   message
          producer
                                                                                 OpenSearch
```


## SNS message filtering pattern
*(Slide 904)*

```text
        Message Filtering - Message filtering ensures that only relevant messages are delivered to relevant
        destination (subscriber), reducing unnecessary processing and improving efficiency.


                                                                                 On-call PagerDuty support endpoint
                                                                                              (HTTP)



                                   Publish      Topic
         Customer                  message                   Priority = Medium
                                                                                 Send slack message to support
          Issue
                                              Order placed




                                                                                 Support Queue
```


## Amazon EventBridge
*(Slide 905)*

- Enables event-based communication between applications and services without requiring point-to-point integrations
- Events generated by a source are routed to one or more destinations based on defined rules, enabling loose coupling and event-driven architectures Lambda
CodeBuild S3
Rules
Rules Rules Rules App                                                                                                  SNS
Kinesis Event Bus Events (JSON objects)


## Amazon EventBridge – Types of event buses
*(Slide 906)*

Default Event Bus                   Partner Event Bus                Custom Event Bus
- Automatically created per AWS
- Used to receive events directly
- Created for specific use cases account; receives events from all      from integrated SaaS partner          to receive events from custom supported AWS services                 applications.                         applications or services, including cross-account scenarios.
App App Default                          SaaS Event Event Bus                                                                  Custom Bus                                  Event Bus More...                                  More...
AWS SaaS AWS Services                                 Partners


## Amazon EventBridge - Features
*(Slide 907)*

- Integrates with 130+ event sources and 40+ targets such as SNS, SQS, and Kinesis.
- Integration without writing custom code, integrates with many SaaS platforms without requiring web-hooks.
- Handles retries with exponential backoff for up to 24 hours for the failed deliveries.
- Rules for event filtering and event transformation before delivery Amazon EventBridge
- Supports schema registries to discover, versioning, and reuse event schemas across applications.
- Allows event archiving and replays which is useful for reprocessing, disaster recovery and debugging.


## Amazon EventBridge Rules                                                             {
*(Slide 908)*

"detail-type": ["OrderEvent"], "detail": { "status": [“Confirmed"] },
- Rules defined in EventBridge determine when and how events are                     .. .. routed to targets.                                                                 .. }
- A rule matches incoming events (from AWS services, custom apps, or SaaS) based on patterns you define.                                                                                    Lambda
- Uses event pattern filtering (JSON pattern matching) to select only relevant events.
- A single rule can target multiple destinations (e.g., Lambda, SQS, SNS,                                                CodeBuild Step Functions, API Gateway, EventBridge buses).
- Multiple rules can match the same event enabling fan-out.                                                               SNS
- You can have catch-all type rule to send events to default target
- Supports input transformations (e.g. OrderNumber -> OrderId)                                                           Kinesis Rules events EventBridge Rule types:
- Schedule-Based Rules: Trigger actions based on time schedules
- Event Pattern–Based Rules: Trigger when an event matches a defined event pattern


## Sample EC2 API event
*(Slide 909)*

```text
                                                                     {                                                          Rule
                                                                         "source": ["aws.ec2"],
                                                                         "detail-type": ["EC2 Instance State-change Notification"],
                                                                         "detail": {
   {                                                 EC2 API Event         "state": ["stopped"]
       "version": "0",                                                   }
       "id": "abcd1234-5678-9012-efgh-345678901234",                 }                                                     Lambda
       "detail-type": "EC2 Instance State-change Notification",
       "source": "aws.ec2",
       "account": "123456789012",                                                                                          CodeBuild
       "time": "2023-11-14T10:20:30Z",
       "region": "us-east-1",
       "resources": [
           "arn:aws:ec2:us-east-1:123456789012:instance/i-                                                                  SNS
   0abcd1234ef567890"                                                                             Rules
       ],                                                            EC2 API
       "detail": {                                                                                        API
                                                                      Event                                                 Kinesis
           "instance-id": "i-0abcd1234ef567890",
           "state": "stopped"
       }
   }

                                                                                                                  Zendesk ticket
                                                                                                                 creation endpoint
```


## Rules with advanced matching/filters
*(Slide 910)*

- Prefix matching { "detail": { "username": [{ "prefix": "admin" }] } }
- Anything-but matching { "detail": { "status": [{ "anything-but": ["failed", "cancelled"] }] } }
- Numeric matching { "detail": { "amount": [{ "numeric": [">", 100] }] } }
- IP address matching { "detail": { "sourceIp": [{ "cidr": "10.0.0.0/16" }] } }
- Exists/Not Exists matching { "detail": { "errorCode": [{ "exists": true }] } }
- AND/OR logic { "detail": { "status": ["created", "cancelled"] } }


## Exercise - Send notification when EC2 instance is launched
*(Slide 911)*

```text
                                                       1    Create EventBridge Rule for EC2-state-
                                                            change event using default event bus
                                                            Target should be the SNS topic that you
                                               Topic   2
                                                            created earlier

                                                       3    Launch a test EC2 instance. Verify if you
                                                            receive the message over an email or SMS.



    EC2 APIs                EventBridge       SNS      Note: In some countries there are restrictions on sending
                                                       SMS like SMS are delivered only during specific time of
                                                       the day or promotional messages are blocked etc.
```


## EventBridge Schema Registry
*(Slide 912)*

Schema registry stores event structure to enable consistent communication between the event producers and event consumers thereby reducing integration errors.
- Helps developers understand event formats without                    Schema Registry manually inspecting payloads.                                  Schema Versions
- Supports multiple schema types: OpenAPI, JSON, Apache Avro etc.
- Can generate code bindings (Java, Python, TypeScript, etc.) Producer              Event Bus      Consumer
- Schemas include versioning, allowing evolution of event formats over time.


## Event Archiving and Replay
*(Slide 913)*

EventBridge can store (archive) a copy of all events that pass through an event bus so you can view, replay, and debug them later.
- You can archive all events or only events matching specific rules.
- Retention period up to 7 years.
- Replayed events maintain the original event structure and timestamp.
- Useful for debugging, auditing, or recovering from downstream failures.
Image source: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-archive.html


## Amazon EventBridge - Key points
*(Slide 914)*

- Amazon EventBridge is a serverless event bus that routes events from AWS services, SaaS applications, and custom apps to targets using rules.
- It supports schedule-based events and event pattern–based routing, allowing you to deliver events only to the services that need them.
- Rules provide powerful content-based filtering, including prefix matching, numeric filters, anything-but matching, exists checks, and IP/CIDR matching.
- Targets can include Lambda, SQS, SNS, Step Functions, API destinations, Event buses, and more.
- EventBridge offers a Schema Registry to automatically discover event structures, version them, and generate code bindings for strongly typed development.
- It supports event archiving, allowing you to retain events for up to 7 years and replay them later for debugging, recovery, or onboarding new consumers.
Overall, EventBridge helps build loosely coupled, event-driven architectures by routing the right events to the right services with minimal integration effort.


# AWS Data Security Services

*(Source: Slide 915)*

### AWS Data Security

*Securing data in transit, at rest and secrets*


## Data Security
*(Slide 916)*

```text
                                                                Data at Rest




                                              Data In Transit
                 A                                                  B
```


## Certificate
*(Slide 917)*

```text
                                                                                              Authority (CA)
      HTTPS and SSL/TLS
                                              HTTPS Traffic


     https://somebank.com                                                                    somebank.com




                                                              https://youtu.be/cLYv4uSFJA8



                                                                               Public Key


                                                                                                  Private Key
```


## Public vs Private SSL/TLS certificate
*(Slide 918)*

Public Certificate                              Private Certificate
- Issued by an internal private CA.
- Issued by globally trusted Certificate Authorities (CAs) like DigiCert, Let’s Encrypt, Symantec, Amazon ATS.          •   Not trusted by browsers unless the private CA’s root certificate is installed manually.
- Trusted by all major browsers and operating systems automatically.                                               •   Used for communication between internal systems,
- Used for internet-facing websites and APIs.                      microservices, intranets, VPNs or private IoT devices e.g. Connected cars
- Require domain ownership validation (DNS, HTTP, or email).                                                      •   Can issue both server and client certificates (mTLS).
- Usually have longer validity (months/years)
- Customizable certificate policies, fields, and lifetimes.
- Useful for short-lived certificates in zero-trust setups.
Trust store Web                                                                  IoT Application                                                          Application Client                                                              IoT Gateway


## Amazon Certificate Manager (ACM)
*(Slide 919)*

- Provisions, stores, and renews Public and Private SSL/TLS certificates (X.509) used for securing websites and applications.
- ACM can issue wildcard certificates (e.g., *.example.com) for flexible subdomain coverage.
- Public certificates are free for use with AWS services like CloudFront, ALB, API Gateway, App Runner, EKS, BeanStalk etc.                             Public Cert         Private Cert
- ACM allows exporting the Public certificates to use with your own application or EC2 instances (additional cost per certificate). Amazon Certificate
- ACM auto-renews certificates before expiry. No manual steps required        Manager (ACM) as long as validation (DNS/email) remains intact.
- To issue and manage Private Certificates, ACM natively integrates with Amazon Private Certificate Authority (Private CA)
- ACM allows importing the 3rd party certificates                                                     Private Cert Amazon Private Certificate Authority (AWS PCA)


## Importing Public Certificate into ACM
*(Slide 920)*

- You can import certificates created outside AWS (e.g., Let’s Encrypt, DigiCert, OpenSSL).
- You must provide three components:
  - Certificate
  - Private key                                                            import
  - Certificate chain (intermediate CA certificates)
- You can attach imported certificates to AWS services just like ACM-issued certs.
- Imported certificates do not auto-renew. You must re-import the certificate before expiry.                                        Amazon Certificate Manager
- ACM sends expiration notification starting # days before expiry (default: 45 days)


## ACM Certificate Lifecycle events
*(Slide 921)*

- ACM sends daily expiration events for all active certificates (public, private and imported) starting 45 days prior to expiration.
- These events are automatically published to Amazon EventBridge (no setup required).
- Events include status changes such as:                                                                        Lambda
  - Cert. Expiration approaching event
  - Cert. Expired event
  - Cert. Renewal action required event                                                                        SQS ACM       EventBridge
  - more..
- You can create EventBridge rules to trigger actions when these events occur (Lambda, SNS, Slack alerts, ticketing systems).                                                          SNS
Action Helps avoid downtime caused by expired or validation-failed certificates.


## ACM Certificates – Common usage
*(Slide 922)*


## ACM with ELB
*(Slide 923)*

- Used when ELB to be accessed with custom domain name and HTTPS (ALB) or TLS (NLB)
- ACM certificate to be issued in the ELB region
- Use Route 53 Alias record to point custom domain                      80 name to ELB DNS ACM
EC2
HTTPS/TLS              443                80
Client ELB              EC2 (ALB, NLB, CLB)
SSL /TLS Termination Webserver


## ACM with API Gateway
*(Slide 924)*

- Used to enable HTTPS with custom domain names for API Gateway APIs.
- For regional API gateway, ACM certificate must be in the same region as the API
- For Edge-optimized APIs, ACM certificate must be in ACM us-east-1 (N. Virginia) region.                                           ALB
HTTPS/TLS              443
Client API Gateway      EC2
SSL /TLS Termination API backend


## ACM with CloudFront
*(Slide 925)*

- Used when CloudFront to be accessed with alternate domain name and HTTPS
- ACM certificate to be issued in the us-east-1 (N. Virginia) region
- Use Route 53 Alias record to point custom domain ACM ALB name to CloudFront distribution DNS
HTTPS
Client CloudFront   EC2
SSL /TLS Termination S3


## Amazon Private CA (PCA)
*(Slide 926)*

- Acts as a fully managed Certificate Authority, so you don’t manage HSMs, hardware, or CA servers.
- Creates private, internal-use certificates used for intranet, internal microservices, on-prem systems, IoT devices, VPN users.
- You pay a monthly fee per CA plus per-certificate issuance. Unlike ACM public certs, these are not free.
- Supports hierarchical CA structure - Root and subordinate CAs to match enterprise PKI architecture. Amazon Private
- Enables mTLS (mutual TLS)                                                       Certificate Authority
- You can enforce policies, extensions, key types, and constraints through templates.


## Encrypting data at rest
*(Slide 927)*


## Data at rest – Encryption and Decryption
*(Slide 928)*

```text
                                              Encryption Key                   Decryption Key




               Plain Text                       Encryption     Cipher Text       Decryption      Plain Text
                 Data                           Algorithm         Data           Algorithm         Data


                           MY NAME IS CHETAN                                 NZ OBNF JT DIFUBO
```


## Data at rest – Symmetric encryption
*(Slide 929)*

Use cases
- Disk encryption
- Data stored in the databases Same key
- Data sent over SSL channel
Plain Text                     Encryption          Cipher Text         Decryption                 Plain Text Data                         Algorithm              Data             Algorithm                    Data
AES-256 (Advanced Encryption Standard, 256-bit)


## Data at rest – Asymmetric Encryption
*(Slide 930)*

Use cases Keypair                         •  Digital Certificates
- Digital Signature Public Key                             Private Key
Plain Text                          Encryption         Cipher Text         Decryption                   Plain Text Data                              Algorithm             Data             Algorithm                      Data
RSA (Rivest–Shamir–Adleman) or ECC (Elliptic Curve Cryptography)


## AWS KMS
*(Slide 931)*


## AWS Key Management Service (KMS)
*(Slide 932)*

- AWS KMS is a data encryption and key management service
- Supports both Symmetric and Asymmetric keys S3        EBS          RDS DynamoDB
- Supports various encryption algorithms (e.g. AES, ECC etc.) and keys (RSA-2048, ECC NIST P-256 etc.) Should have IAM perm to access KMS key
- Keys never leave KMS - Operations happen inside AWS- managed HSMs (FIPS 140-2 validated).
- User or AWS service needs IAM permissions to use KMS service and encryption keys (called Key policy). KMS
- KMS keys can be shared with other AWS accounts.
- KMS keys are regional, but you can also create Multi-region                            Customer AWS Managed KMS keys to replicate keys across regions for DR, backup etc.                        Managed Keys Keys


## Okay, got it… but
*(Slide 933)*

```text
                                              Does KMS perform the actual Data encryption?

                                              For example, encrypting each S3 object or EBS volume or
                                              snapshots or DynamoDB table data?



                                                                          No


                               AWS Service performs the encryption using Envelope Encryption method.
```


## Envelope encryption
*(Slide 934)*

- Most of the AWS Services (e.g. S3, EBS, RDS, DynamoDB etc.) use Envelope encryption
- Using the KMS Key (CMK), KMS generates a Data Key and returns:
  - Plaintext key → for encryption Requests Data Key
  - Encrypted key → safe to store
- AWS Service encrypts your data locally                           KMS
- Uses the plaintext data key inside their own infrastructure to         Receives Plain text Data encrypt the data at huge scale and high speed. This happens on         Key + encrypted Data key the storage nodes, not inside KMS.
- Plaintext key is immediately discarded                                 Encrypt Object using Data Key
- Only the encrypted (KMS-protected) key is stored next to the encrypted data. Stores Encrypted data and encrypted data key


## KMS Key policy
*(Slide 935)*

{ "Version": "2012-10-17", "Statement": [ KMS keys comes with the key policy which controls who { can use or manage a KMS key (like S3 bucket policies.)              "Sid": "AllowKeyAdminAccess", "Effect": "Allow", "Principal": { "AWS": You can grant different level of access to the key such as:   "arn:aws:iam::123456789012:role/AdminRole" },
- Administrator – Manage keys (Rotate, manage                       "Action": [ permissions)                                                         "kms:DescribeKey", "kms:Create*",
- Key Users – Encrypt/decrypt and generate Data Keys                   "kms:Enable*", "kms:Disable*",
- Cross-account access                                                 "kms:ScheduleKeyDeletion", "kms:CancelKeyDeletion", "kms:Put*", "kms:Update*", "kms:List*", "kms:Revoke*" ], When sharing the resources with other AWS account e.g.             "Resource": "*" AMIs or encrypted snapshots, KMS key policy should be           } updated to allow KMS key access to the target AWS account       ] }


## KMS Multi-region Keys
*(Slide 936)*

- Designed for cross-region workloads
- Consists of Primary key and Replica keys
- Replicas share identical key material and key ID but different ARNs.
- Automatic synchronization of key metadata, policy, tags, and rotation state              Primary from primary to replicas.
- No re-encryption required. Encryption/decryption happens in the same                    Sync region as the data.
Replica              Replica


## KMS Multi-region Keys
*(Slide 937)*

- Key ID remains same                                               Region                     Primary
- Key Material remains same
- Key ARN is different (region code)
arn:aws:kms:us-east-1:111122223333:key/mrk- 1234abcd12ab34cd56ef12345678990ab
Region                       Replica                Region                     Replica
arn:aws:kms:us-west-2:111122223333:key/mrk-        arn:aws:kms:eu-west-1:111122223333:key/mrk- 1234abcd12ab34cd56ef12345678990ab                  1234abcd12ab34cd56ef12345678990ab


## Multi-region key scenarios
*(Slide 938)*

- S3 Bucket Replication across Regions (CRR)
- DynamoDB Global Tables
- Aurora Global Database                                          S3 CRR
- EBS Snapshot copy across regions
- AMI copy across regions                                  DynamoDB Global Tables
Aurora Global Database
When sharing the resources with other AWS account     EBS Snapshot Copy e.g. AMIs or encrypted snapshots, KMS key policy should be updated to allow KMS key access to the target AWS account                        AMI Copy


## AWS Secrets Manager
*(Slide 939)*

- Helps you to securely store, retrieve and rotate credentials for your databases and other services.
- You can store passwords, API keys, Tokens or any other login RDS credentials. 3 access                         KMS
- No need to hardcode credentials in the application code.
- Rotate credentials automatically for integrated services (e.g. RDS - 1 MySQL, PostgreSQL, Aurora etc. ) or invoke Lambda function to                      get DB password rotate the credentials.                                                      APP
password
- Secrets are encrypted using KMS                                             EC2        2 Secrets Manager
- Access to secrets is controlled using IAM permissions


## Multi-region secrets
*(Slide 940)*

- You can replicate your secrets in multiple AWS Regions to support applications spread across those Regions to meet Regional access and low latency requirements.                  Region    Region
- You can promote a replica to a primary during failover.
- Secret rotation happens in the primary region only; replicas ******    ****** automatically receive updated values.                          Primary   Replica
Use cases:
- EKS/ECS clusters in multiple regions
- Multi-region Lambda/API workloads


## Secrets Manager vs SSM Parameter Store
*(Slide 941)*

✓ Secrets Manager is designed for secrets whereas Parameter Store is designed for configuration.
Secrets Manager                          SSM Parameter Store
What is it?                Allows storing and rotating secrets      Allows storing configuration parameters (passwords, keys, tokens)                     (strings, JSON, paths) Rotation                       Built-in secrets rotation                      Not supported
Multi-region support               Multi-region with secret replicas                   Not supported
Encryption                  Secrets are encrypted by default                  Optional encryption
- Database passwords
- App settings
- OAuth tokens
- Feature flags
- Third-party API keys
- App versions
- Certificates or private keys
- Non-sensitive configuration


# Infrastructure as Code (AWS CloudFormation)

*(Source: Slide 942)*

### Infrastructure as Code

*AWS CloudFormation and CDK*


## Infrastructure and application deployment..
*(Slide 943)*

```text
                  Infrastructure
                     as Code

                                                                          AWS CloudFormation     AWS CDK




               DevOps – CI/CD

                                              CodeCommit      CodeBuild       CodeDeploy         CodePipeline




                     Application
                     Deployment
                                                           ElasticBeanstalk   Amazon Lightsail      Amplify
```


## Why Infrastructure as Code?
*(Slide 944)*


## Using AWS CLI
*(Slide 945)*

```text
              aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications ResourceType=vpc,Tags=[{Key=Name,Value=MyVPC}]
              aws ec2 create-internet-gateway
              aws ec2 attach-internet-gateway --vpc-id vpc-xxxxxxxxxx --internet-gateway-id igw-xxxxxxxxxx
              aws ec2 create-subnet --vpc-id vpc-xxxxxxxxxx --cidr-block 10.0.0.0/24 --availability-zone ap-south-1a
              aws ec2 create-subnet --vpc-id vpc-xxxxxxxxxx --cidr-block 10.0.1.0/24 --availability-zone ap-south-1b
              aws ec2 create-subnet --vpc-id vpc-xxxxxxxxxx --cidr-block 10.0.11.0/24 --availability-zone ap-south-1a
              aws ec2 create-subnet --vpc-id vpc-xxxxxxxxxx --cidr-block 10.0.12.0/24 --availability-zone ap-south-1b
              aws ec2 create-route-table --vpc-id vpc-xxxxxxxxxx
              aws ec2 create-route --route-table-id rtb-xxxxxxxxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxxxxxxxx
              aws ec2 associate-route-table --route-table-id rtb-xxxxxxxxxx --subnet-id subnet-xxxxxxxxxx
              aws ec2 create-route-table --vpc-id vpc-xxxxxxxxxx
              aws ec2 associate-route-table --route-table-id rtb-xxxxxxxxxx --subnet-id subnet-xxxxxxxxxx
              aws ec2 allocate-address --domain vpc
              aws ec2 create-nat-gateway --subnet-id subnet-xxxxxxxxxx --allocation-id eipalloc-xxxxxxxxxx
              aws ec2 create-route --route-table-id rtb-xxxxxxxxxx --destination-cidr-block 0.0.0.0/0 --gateway-id nat-xxxxxxxxxx
              …
```


## Using AWS SDK (python – boto3)
*(Slide 946)*

```text
           import boto3                                                                                 vpc.py
           ec2 = boto3.client("ec2", region_name="ap-south-1")

           # VPC
           vpc = ec2.create_vpc(CidrBlock="10.0.0.0/16")["Vpc"]["VpcId"]
           ec2.create_tags(Resources=[vpc], Tags=[{"Key":"Name","Value":"MyVPC"}])

           # Internet Gateway
           igw = ec2.create_internet_gateway()["InternetGateway"]["InternetGatewayId"]
           ec2.attach_internet_gateway(VpcId=vpc, InternetGatewayId=igw)

           # Subnets
           subnets = []
           for cidr, az in [("10.0.0.0/24","ap-south-1a"),("10.0.1.0/24","ap-south-1b"),
                       ("10.0.11.0/24","ap-south-1a"),("10.0.12.0/24","ap-south-1b")]:
              subnets.append(ec2.create_subnet(VpcId=vpc, CidrBlock=cidr,                                        $python vpc.py
           AvailabilityZone=az)["Subnet"]["SubnetId"])

           # Public route table
           rtb_public = ec2.create_route_table(VpcId=vpc)["RouteTable"]["RouteTableId"]
           ec2.create_route(RouteTableId=rtb_public, DestinationCidrBlock="0.0.0.0/0", GatewayId=igw)
           ec2.associate_route_table(RouteTableId=rtb_public, SubnetId=subnets[0])

           # NAT Gateway
           eip = ec2.allocate_address(Domain="vpc")["AllocationId"]
           nat = ec2.create_nat_gateway(SubnetId=subnets[0], AllocationId=eip)["NatGateway"]["NatGatewayId"]
           …
```


## Ways to deploy AWS infrastructure
*(Slide 947)*

```text
                         AWS Console




                              AWS CLI


C++, go, java, Java script,
Kotlin, .Net, Node.js, PHP,
python, ruby, rust, swift


                          AWS SDK




     AWS CloudFormation                       CDK
```


## Using AWS CloudFormation
*(Slide 948)*

```text
       Template    CloudFormation
    (JSON or YAML)




         DevOps
```


## Ways to deploy AWS infrastructure
*(Slide 949)*

```text
                         AWS Console




                              AWS CLI


C++, go, java, Java script,
Kotlin, .Net, Node.js, PHP,
python, ruby, rust, swift


                          AWS SDK




                                                    3rd Party/
     AWS CloudFormation                             Opensource
                                              CDK
           (YAML or JSON)
```


## AWS CloudFormation
*(Slide 950)*


## Using AWS CloudFormation
*(Slide 951)*

```text
       Template    CloudFormation
    (JSON or YAML)




         DevOps
```


## AWS CloudFormation
*(Slide 952)*

```text
       AWS CloudFormation is a declarative way of outlining your AWS Infrastructure for any resources.

        For example, within a CloudFormation template, you say:
     ✓ I want a VPC and Subnets
     ✓ I want an internet gateway and attach it to the VPC
     ✓ I want a security group
     ✓ I want two EC2 machines using this security group in the
       subnet just created




    Then CloudFormation creates those for you, in the right order, possibly in parallel and with the exact
    configuration that you specify.
```


## Benefits of using AWS CloudFormation
*(Slide 953)*

- AWS resources are created & deleted automatically, hence no manual errors
- Resources are created and deleted as a group (called Stack), hence no ghost resources on stack deletion
- AWS resources which are not dependent, are created in the           AWS         Infrastructure parallel which speeds up infrastructure deployment             CloudFormation     Composer significantly.
- Same CloudFormation template can be deployed in different AWS regions or accounts with minimal or no changes. Templates can be easily shared among teams.
- The template can be version controlled (using git, svn etc.) so it’s easy to go back to previous deployment
- Supports creating CloudFormation template visually (drag/drop) using Infrastructure composer


## AWS CloudFormation – Important to know
*(Slide 954)*

- CloudFormation Template structure
- Nested Stacks
- StackSets
- Change Sets and Drift Detection
- Automatic Rollback and Deletion behavior


## CloudFormation Template structure
*(Slide 955)*

AWSTemplateFormatVersion: "2010-09-09"
- AWSTemplateFormatVersion: 2010-09-09                                             Description: Sample CloudFormation template for SAA exam preparation
- Description: Text that describes the template Metadata:
- Metadata                                                                           Author: Chetan Purpose: Demonstrate CFN Template sections ✓ Additional information to improve usability e.g. parameter grouping for UI                                                                        Parameters: Env:
- Parameters Type: String ✓ Input values that you can pass when creating stacks                           Default: dev AllowedValues: [dev, prod] ✓ Can have default values and allowed values (with patterns)                    Description: Deployment environment
- Mappings Mappings: ✓ Fixed variables (key-value pairs) defined for lookup                        InstanceConfig: ✓ Common use: AMI IDs per region, instance sizes per env                        dev: { InstanceType: t2.micro } prod: { InstanceType: t3.small }
- Conditions ✓ Add flexibility to the template by evaluating conditions                  Conditions: IsProd: !Equals [ !Ref Env, prod ] ✓ Use condition Intrinsic functions: Fn::And, Fn::Or, Fn::If etc.           … ✓ Example: If env=prod create EBS volume of size 1TB or 100GB               …


## CloudFormation Template structure
*(Slide 956)*

Resources: EC2Instance: Type: AWS::EC2::Instance
- Resources (Mandatory)                                           Properties: ImageId: ami-0abcdef1234567890 ✓ Declares AWS resources to include in the stack             InstanceType: !FindInMap [ InstanceConfig, !Ref Env, InstanceType ] ✓ Example: EC2 instances, S3 buckets, RDS databases)         Tags: - Key: Name
- Outputs                                                                Value: !Sub "${Env}-ec2" - Key: Environment Value: !Ref Env ✓ Values returned after stack creation                          - !If - IsProd ✓ Can be exported for cross-stack references                      - { Key: Critical, Value: "true" } - !Ref AWS::NoValue ElasticIP: Type: AWS::EC2::EIP Properties: Domain: vpc EIPAssociation: Type: AWS::EC2::EIPAssociation Properties: InstanceId: !Ref EC2Instance EIP: !Ref ElasticIP Outputs: ElasticIP: Description: Elastic IP address Value: !Ref ElasticIP


## CloudFormation template examples
*(Slide 957)*

```text
        AWSTemplateFormatVersion: '2010-09-09'
        Description: Launch an EC2 instance with SSH access

        Resources:
         EC2Instance:                                                  Upload to S3 bucket
          Type: 'AWS::EC2::Instance'
          Properties:
           InstanceType: t2.micro
           KeyName: !Ref KeyName
           ImageId: ami-0c55b159cbfafe1f0 # Replace with your AMI ID
           SecurityGroups:
             - !Ref InstanceSecurityGroup

         InstanceSecurityGroup:
           Type: 'AWS::EC2::SecurityGroup'                                Create Stack
           Properties:
            GroupDescription: Allow SSH
            SecurityGroupIngress:
             - IpProtocol: tcp
               FromPort: 22
               ToPort: 22
               CidrIp: 0.0.0.0/0

        Parameters:                                                       Region
         KeyName:
          Description: Name of an existing EC2 KeyPair
          Type: String                                                      VPC
        Outputs:
         InstanceId:
           Value: !Ref EC2Instance
         PublicIP:                                                                 EC2
           Value: !GetAtt EC2Instance.PublicIp
```


## Sample architecture..
*(Slide 958)*

```text
                                                                                             Users


                                                                                          Internet
                                                                                          gateway
                                                         Availability Zone                                  Availability Zone
                                              Public subnet                                       Public subnet




                                                                             Load Balancer
                                              Private subnet                                      Private subnet
                  Web /
                  App Tier                                                   Auto Scaling group

                                                               Instance                                            Instance

                                              Private subnet                                      Private subnet

                  Database
                    Tier                                                        Replication
                                                         Amazon RDS                                           Amazon RDS
```


## Let’s create VPC and related resources..
*(Slide 959)*

```text
                                                                                     Internet
                                                                                     gateway                           10.10.0.0/16
                                                        Availability Zone                          Availability Zone
                                                                                                                                 Destination     Target
                                              Public subnet           10.10.1.0/24       Public subnet            10.10.2.0/16
                                                                                                                                 10.10.0.0/16    local

                                                                                                                                 0.0.0.0/0       Igw-xxxxx


                                              Private subnet       10.10.11.0/24         Private subnet          10.10.12.0/24
                                                                                                                                 Destination    Target

                                                                                                                                 10.10.0.0/16   local
```


## Exercise – Create VPC using CloudFormation
*(Slide 960)*

1. Download a CloudFormation template: https://github.com/awswithchetan/aws-solutions-architect- associate/blob/main/vpc.yaml
2. Use this template to create a CloudFormation stack
Region
VPC
Stack Template                AWS CloudFormation


## Nested Stack
*(Slide 961)*

- A parent stack creates and manages one or more child (nested) stacks. Root Stack
- Useful for modularity, reuse, and managing template size limits.
- Common exam use cases:
  - Reusing VPC, IAM, or security baseline templates
  - Separating networking, compute, and database layers


## Example: Spitting the template using nested stack
*(Slide 962)*

*Source: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-nested-stacks.html*


## StackSets
*(Slide 963)*

AWS Organization
Account X (Administrator)
- StackSets let you deploy same CloudFormation stacks across multiple AWS accounts and multiple Regions.
- Commonly used with AWS Organizations for centralized management.
- Creates stack instances in target accounts and Regions.                                              StackSet
- Updates to a StackSet can be rolled out automatically to all stack instances.
- Supports drift detection across accounts and Regions.                    Region 1                                Region 2
- Provides failure tolerance and deployment order controls.                       Account A                               Account C
- Used for organization-wide resources like IAM roles, Config rules, and security baselines. Stack                                   Stack
Account B                               Account D
✓ Nested stacks = modular templates in one account/Region                                 Stack                                   Stack ✓ StackSets = same template across many accounts/Regions


## Change Sets and Drift Detection
*(Slide 964)*

CloudFormation Change Sets
- Change sets let you preview changes before updating a stack.
- Show which resources will be added, modified, or deleted.
- Help avoid unintended resource replacement or deletion.
- Does not make changes until explicitly executed. Update       Review   Execute
Current    Change Set                 Updated Template                                Stack


## Change Sets and Drift Detection
*(Slide 965)*

Drift Detection
- Drift detection identifies manual changes made outside CloudFormation.
- Compares the actual resource configuration with the template.
- Marks stacks/resources as IN_SYNC or MODIFIED.
- Helps detect configuration drift and enforce IaC discipline.
✓ Change Sets = Review impact before applying stack updates ✓ Drift Detection = Identify the resources modified outside of CloudFormation Source: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/detect-drift- stack.html


## Automatic Rollback and Deletion behavior
*(Slide 966)*

Rollback:
- Rollback occurs automatically if stack creation or update fails.
- During rollback, CloudFormation attempts to return the stack to the last known stable state.
- For stack creation failures, all successfully created resources are deleted by default.
- You can disable Rollback during stack creation (useful for troubleshooting).
Resource Deletion Policy:
  - Retain – Resource is kept after stack deletion. Prevent accidental deletion of data                                                         Resources: MyDB:
  - Snapshot – Creates snapshot (for supported resources like RDS).              Type: AWS::RDS::DBInstance DeletionPolicy: Retain
  - Delete – default behavior.                                                   …


## CloudFormation IAM Service Role
*(Slide 967)*

- CloudFormation does not have permissions by default to create resources.                                                     iam:PassRole
- It uses the permissions of the IAM user or role that launches         cloudformation:* the stack OR uses CloudFormation service role.                        IAM user/role permissions
- iam:PassRole allows an IAM principal to pass a Service Role to the CloudFormation service.
- If CloudFormation service needs to further create AWS           ec2:RunInstances s3:CreateBucket resources (e.g. EC2 instance with IAM role) then Service role   lambda:CreateFunction should also include iam:PassRole permssions.                    more.. iam:PassRole CloudFormation Service Role more.. permissions Stack


## AWS CloudFormation - Summary
*(Slide 968)*

- AWS-native Infrastructure as Code service to provision and manage AWS resources consistently.
- Uses JSON/YAML template to define the desired state of infrastructure
- Template contains different sections like Resources (mandatory), Parameters, Mappings, Conditions, Outputs etc.
- Nested stacks allow large templates to be split into smaller, reusable components managed by parent template.
- StackSets enable deploying the same CloudFormation template across multiple AWS accounts and Regions
- StackSets are commonly used with AWS Organizations for centralized governance.
- Change sets let you preview which resources will be added, modified, or deleted before executing the changes.
- Drift detection identifies changes made outside CloudFormation by comparing actual resources with the template.
- CloudFormation automatically rolls back failed stack creations or updates to the last stable state.
- DeletionPolicy controls resource behavior during rollback or stack deletion. Supports Retain, Snapshot & Delete actions.
- CloudFormation can use a IAM Service Role to create/update/delete resources.
- IAM user/role should have iam:PassRole permissions to pass Service Role to CloudFormation.


## AWS CDK
*(Slide 969)*


## AWS Cloud Development Kit (CDK)
*(Slide 970)*

```text
           An open-source software development framework to define your cloud application resources using familiar
           programming languages.




           Source Code (TypeScript,                 Templates      AWS CloudFormation              Cloud Resources (Stack)
             Python, and Java etc.)




                       cdk init               npm run build     cdk synth               cdk diff            cdk deploy
```


## Using AWS CDK
*(Slide 971)*

```text
       Template    CloudFormation
    (JSON or YAML)




          DevOps                  CDK
                                              Write code in language
                                                     of choice
```


## CDK code example
*(Slide 972)*

```text
        from aws_cdk import (                                             ec2-instance-cdk.py
           aws_ec2 as ec2,
           core
        )
        class EC2InstanceStack(core.Stack):
           def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
             super().__init__(scope, id, **kwargs)

             # VPC where the instance will be launched
             vpc = ec2.Vpc.from_lookup(self, "VPC", is_default=True)

             # Security Group to allow SSH access                                               $npm install -g aws-cdk
             security_group = ec2.SecurityGroup(self, "InstanceSG",
               vpc=vpc,
               description="Allow SSH access",
               allow_all_outbound=True
                                                                                                $mkdir ec2-instance-cdk
             )
             security_group.add_ingress_rule(
               peer=ec2.Peer.any_ipv4(),                                                        $cd ec2-instance-cdk
               connection=ec2.Port.tcp(22),
               description="Allow SSH access from anywhere"
             )                                                                                  $cdk init app --language python
        instance = ec2.Instance(self, "EC2Instance",
               instance_type=ec2.InstanceType("t2.micro"),
               machine_image=ec2.MachineImage.latest_amazon_linux(),
               vpc=vpc,                                                                         $pip install aws-cdk.aws-ec2
               security_group=security_group,
               key_name="your-key-pair-name" # Replace with your key pair name
             )                                                                                  $cdk deploy
             # Output the instance ID and public IP
             core.CfnOutput(self, "InstanceId", value=instance.instance_id)
             core.CfnOutput(self, "InstancePublicIP", value=instance.instance_public_ip)

        app = core.App()
        EC2InstanceStack(app, "EC2InstanceStack")
        app.synth()
```


## Application Deployment Services
*(Slide 973)*

*AWS Elastic Beanstalk and Amazon Lightsail*


## In this section..
*(Slide 974)*

```text
                   Infrastructure
                      as Code

                                                                       AWS CloudFormation    AWS CDK




                DevOps – CI/CD

                                              CodeCommit   CodeBuild        CodeDeploy      CodePipeline




                     Application
                     Deployment
                                                                       Elastic Beanstalk   Amazon Lightsail
```


# Application Deployment

*(Source: Slide 975)*


## What developers want?
*(Slide 976)*

```text
                                              Build



                                                      Application

             Developer
```


## What developers want?
*(Slide 977)*

```text
                                                                            Application



                                              Build and Run
                                                                    Managed                  Virtual Server to run small
                                                                    Application              apps and websites
                                                                    Platform


             Developer


                                                              AWS Elastic           Amazon Lightsail
                                                               Beanstalk
```


## AWS Elastic Beanstalk
*(Slide 978)*

- AWS Elastic Beanstalk is a Platform as a Service (PaaS)
- Developers can deploy applications quickly without managing infrastructure details.
- It abstracts underlying infrastructure such as EC2, load balancers, Auto Scaling groups, and scaling policies.
- Just upload application code, and Elastic Beanstalk automatically provisions, manages, and scales the environment. AWS Elastic Beanstalk
- Supports multiple application platforms including Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker.
- Runs applications on common web servers like Apache, Nginx, Passenger, and IIS.


## AWS Elastic Beanstalk
*(Slide 979)*

```text
                                                                                              Health monitoring




                                       AWS Elastic Beanstalk


               Create                                                             Launch        Manage
              Application                          Upload Version
                                                                                Environment   Environment


                                                           Deploy New Version
```


## AWS Elastic Beanstalk - Platform Support
*(Slide 980)*

Application frameworks          Container platforms        AWS services
- Go
- Docker
- EC2
- Java SE
- ECS
- Elastic Load Balancer
- Java with Tomcat
- Auto-scaling Group
- .NET on Windows Server with
- CloudWatch IIS
- Node.js
- RDS
- PHP
- Route 53
- Python
- CloudFront
- Ruby
- More..


## Elastic Beanstalk Environments
*(Slide 981)*

- Web Server environment
  - Used for HTTP/HTTPS request–response web applications.
  - Supports capacity modes:
    - Single-Instance web environment
    - Load Balanced web environment
- Worker environment
  - Used for background or asynchronous processing.
  - Always process messages from an Amazon SQS queue
  - Do not receive direct HTTP traffic from users
  - Are always load-balanced internally using Auto Scaling


## Web server environment
*(Slide 982)*

Single-Instance                                                       Load-Balanced
Availability Zone 1          Availability Zone 1
Auto-scaling group
EIP             EC2      RDS
CW
- One EC2 instance
- No load balancer
- Lowest cost
- Suitable for dev, test, demos, or very low
- Highly available and Scalable traffic apps                                 •   Use ALB and Autoscaling group
- Suitable for high traffic production apps


## Worker environment
*(Slide 983)*

Web Env
- Used for background / asynchronous processing
- Optionally, automatically provisions an Amazon SQS                                                Queue depth metrics queue to receive work messages.
- EC2 instances poll the SQS queue and process                                    SQS messages.                                                 Availability Zone 1         Availability Zone 1
- Uses Auto Scaling based on SQS queue depth (messages in queue).
- Supports cron.yaml for scheduled background jobs. Auto-scaling group
- If processing fails, messages can be retried or sent to a dead-letter queue (DLQ).
- Typically paired with a web environment that pushes jobs to SQS.                                                                                                       CW
- Best for tasks like image processing, email sending, data transformation, and batch jobs.


## Worker environment – How it works?
*(Slide 984)*

- Elastic Beanstalk runs the SQS daemon process on the worker instances
- Daemon pulls the message from SQS queue and makes HTTP POST request on localhost:80 (configurable)
- EB Application listens and responds to these requests http://localhost:80/ HTTP POST
EB Sqsd           Application Messages    (daemon) SQS


## Elastic Beanstalk deployment methods
*(Slide 985)*

- All at once – Deploys the new version to all instances simultaneously, causing small downtime during deployment.
- Rolling – Updates instances in batches, reducing downtime but temporarily lowering capacity.
- Rolling with additional batch – Launches an extra batch of instances before updating, maintaining capacity during deployment.
- Immutable – Creates a new set of instances with the new version and swaps them after validation, offering the safest deployment.
- Traffic splitting – Gradually shifts a percentage of traffic to the new version while both versions run in parallel.
- Blue/Green - Runs two separate environments (current and new), then switches all traffic to the new environment at once after validation, enabling fast rollback.


## Elastic Beanstalk deployment methods
*(Slide 986)*

```text
               Deployment                     Code deployed   Deployment   Downtime    DNS       Rollback process
                 Method                             to           Time                 Change
                                         Existing instances                  Yes       No         Manual redeploy
                All at Once
                   Rolling               Existing instances                  No        No         Manual redeploy


              Rolling with                New and existing                   No        No         Manual redeploy
            additional batch                 instances

                Immutable                     New instances                  No        No      Terminate new instances


             Traffic Splitting                New instances                  No        No         Reroute traffic and
                                                                                               terminate new instances

                                              New instances                  No        Yes           Swap URL
                Blue/Green
```


## Amazon Lightsail
*(Slide 987)*

- For everything you need to jumpstart your project on AWS. Amazon
- Quickly deploy popular applications                                Lightsail
✓      WordPress, LAMP, Nginx, MEAN, Node.js etc.
- Create preconfigured Virtual Private Server (VPS) or Database or Container service
- Fixed price as per selected bundle
- Use Cases: ✓      Simple web applications
✓      Websites (templates for WordPress, Magento, Plesk, Joomla)
✓      Dev / Test environment


# AWS Monitoring and Logging

*(Source: Slide 988)*

*Amazon CloudWatch, AWS X-ray, AWS Health*


## AWS Monitoring and logging services
*(Slide 989)*

- Amazon CloudWatch
- AWS X-Ray
- AWS Health


## Web
*(Slide 990)*

```text
                                                        Users                                 Browser
                                                                                                                              CloudFront
                       myapp.com on AWS
                                                         Route53            myapp.com                                                             Edge
                                                                                                                                                Locations



                                                                         ELB
                                                                                     Auto
      Admin                                                                         Scaling                                     Lambda

                                                Web                                                                             Video
                                               Server         EC2 E      EC2 E
                                                                    B           B                                               Convert
                                                                    S           S                Rekognition             S3                S3
                            SNS
                                                                                                           AI models                                    QuickSight
                                                                                                           enhancement

                                                App
                                                              EC2 EB     EC2 E
                                               Server                           B
                             SES                                    S           S

                                                                                                     Deploy custom
                                                                                                         model
                                                                                                                     Sagemaker


                            SQS
                                              ElastiCache

                                                                               Neptune        Kinesis                    S3              EMR
                                                                                                                                                            Redshift
                       CloudWatch                  Multi-AZ



                                                              RDS       DynamoDB              Glue
```


## Amazon CloudWatch
*(Slide 991)*


## Amazon CloudWatch
*(Slide 992)*

- Collect and tracks metrics, logs, and events for AWS resources and applications.
- Create custom dashboards for a visual representation of metrics and logs, aiding in monitoring and decision-making.
- Provides real-time and historical data
- Set alarms to automatically trigger actions (e.g., scaling, notifications) based on defined     Amazon CloudWatch thresholds.
- Seamlessly integrates with majority of the AWS services (e.g., EC2, RDS, Lambda) and also supports third-party apps via API.


## How CloudWatch works?
*(Slide 993)*

```text
                AWS Resources                                              Amazon CloudWatch

                                                                             10:22:00 CPUUtilization: 65%                                 Lambda
                                                                             10:22:00 NetworkIn: 8777




                                                                 Metrics
          EC2        EBS        S3                                           10:25:03 EBSReadOps: 409


              More AWS services                                              10:25:06 S3 GetRequests 1005                    Alarm
                                                                                                                                           SNS

                                                                                INFO Sending request to
                                                                                https:/somebank/payment/
                                                                                INFO Initiating payment
                                              CloudWatch Agent




                Corporate                                                       INFO Contacted payment server
                                                                                INFO Server accepted the request.
                                                                                                                                         Autoscaling
                                                                 Logs
                data center                                                     DEBUG: Processing..
                                                                                INFO Received ack. Payment successful.
                                                                                ERROR Could not write to database
                                                                                PURCHASE. Permissions denied.
                                                                                                                         Logs analysis




                                                                                                                         CloudWatch
                                                                                                                         Dashboards
```


## Amazon CloudWatch Features
*(Slide 994)*

- CloudWatch Metrics
- CloudWatch Metric Streams
- CloudWatch Alarms
- CloudWatch Logs
- CloudWatch Logs insights
- CloudWatch Dashboards
- CloudWatch Events -> Now Amazon EventBridge
- CloudWatch Synthetics
- CloudWatch ServiceLens
- CloudWatch Anomaly detection
- CloudWatch RUM
- More..


## CloudWatch Metrics - Basics
*(Slide 995)*

- CloudWatch Metric is the unit of measurement for system performance at a specific time, such as:
    - EC2: CPUUtilization
    - Network: NetworkIn and NetworkOUT Bytes
    - EBS: DiskReads and DiskWrites
    - S3: NumberOfRequests Metrics
  - Metrics are organized into namespaces based on the AWS service. e.g. AWS/EC2, AWS/S3, AWS/EBS, AWS/ELB etc.
  - A dimension is a key-value attribute that identifies a metric (e.g. For AWS/EC2                 CloudWatch namespace, InstanceId is one of the dimension)
  - Metrics data is stored as a timeseries to be able to analyse and act based on metrics values (e.g. raise an alarm when CPUUtilization > 90% for 5 mins)
  - CloudWatch Supports:
    - Basic monitoring – Metrics are published every 5 minutes (default)
    - Detailed monitoring – Metrics are published every 1 minute (enabled manually, additional cost).


## CloudWatch custom metrics
*(Slide 996)*

- CloudWatch supports publishing custom metrics which can be pushed to existing or custom namespaces.
- Custom metrics are commonly used for application-level and business metrics.
$aws cloudwatch put-metric-data --metric-name product1 --namespace revenue --value 15 --timestamp 2026-01- 20T11:30:40.000Z { Namespace: revenue, Metric: product1, Timestamp: 2026-01-20T11:30:40, Value: 15 } Application
Amazon CloudWatch Publish $ metric for every product sold on your website
Product sold < 10 in 24hrs   Alarm


## CloudWatch Metric Streams
*(Slide 997)*

- CloudWatch Metric Streams allow you to continuously stream metrics from CloudWatch to different destinations.
- Metrics are delivered in near real-time with low latency, making them suitable for real-time monitoring and analytics.                                              Amazon S3
- Metrics can be streamed to Amazon Data Firehose as the                  Metric stream delivery service.
- CloudWatch Metric Streams also support third-party             CloudWatch               Amazon Data     Amazon Redshift monitoring tools such as Datadog / Dynatrace / New Relic                                 Firehose /Splunk /Sumo Logic
- You can filter metrics by namespaces to stream only a Amazon selected subset, reducing cost and noise.                                                                OpenSearch
3rd Party SaaS platforms


## Amazon CloudWatch Alarms
*(Slide 998)*

  - CloudWatch Alarms allows you to take action when the metrics fall outside of the levels (high or low thresholds)
  - A CloudWatch Alarm is always in one of three states: OK, ALARM, INSUFFICIENT_DATA
  - Alarms actions
    - Auto Scaling: Increase or decrease EC2 instances “desired”                 Action count.
    - EC2 Actions: Stop, terminate, reboot or recover an EC2        CloudWatch instance.
    - SNS notifications: Send a notification into an SNS topic.
    - More..
- Alarms can evaluate metrics using different statistics such as Average, Sum, Minimum, Maximum, and Percentiles.
- You can also combine multiple alarms to create a Composite Alarm


## Composite Alarm
*(Slide 999)*

- Composite alarms combine multiple CloudWatch alarms using logical rules (AND / OR / NOT).                                                                               EC2
- They don’t monitor metrics directly; they evaluate the state of other alarms.                                                                                               EC2
- Help reduce alert noise by triggering only when meaningful conditions     ALB occur together.
- Actions (SNS, Lambda, OpsCenter, etc.) are triggered only when the                                    EC2 composite condition is met.
- Example:                                                                            AND
  - CPU_Alarm: Avg. EC2 CPU Utilization > 80%
  - ELB_Alarm: Application Load Balancer 5XX errors > 50            ELB_Alarm         CPU_Alarm
  - Composite Alarm: ALARM(CPU_Alarm) AND ALARM(ELB_Alarm) Lambda
SNS Action Composite Alarm                     Ops Center


## Exercise - CloudWatch Metrics and Alarm
*(Slide 1000)*

```text
                                                             1   Create a SNS notification topic and create an
                                                                 email subscription. Confirm subscription by
                                                                 clicking the link received in the email.

                                                             2   Launch an EC2 instance
                                Alarm
                                                             3   Go to AWS CloudWatch -> Alarms -> Create new
                      metrics                                    alarms for your EC2 instance when
                                                                 CPUUtilization > 50 percent

                                                             4   Login into EC2 instance over SSH and load the
EC2 Instance                      CloudWatch    SNS
                                                                 CPU by using stress command


                                                             5   Wait up to 5 mins to see EC2 CPU utilization
                                                                 going high. This should trigger an Alarm.
   Commands to install and run stress to load the EC2 CPU:
                                                             6   Leave the EC2 instance running if you will be
   sudo yum install -y stress                                    continuing with CloudWatch Logs lecture and
   stress --cpu 1 --timeout 600                                  corresponding exercise. Otherwise terminate
                                                                 the instance.
```


## Amazon CloudWatch Logs
*(Slide 1001)*

On-premise
- Collect, store, monitor, and analyze logs from AWS services and applications.         EC2             Server
- Sources: EC2, ECS, Elastic Beanstalk, Lambda, API Gateway, CloudTrail, Route 53, VPC Flow Logs and more AWS services + any custom application                                              CloudWatch CloudWatch
- For collecting logs from EC2 or on-premises server, install CloudWatch Agent.                  Agent             Agent
- Log Structure
  - Log Group: A logical container for logs, usually represents one application             IAM permissions or service. (e.g. /aws/ec2/application/)
  - Log Stream: A sequence of log events, typically represents an EC2 instance, container, Lambda execution, or log file. (e.g. instance_id )
- Log Retention & Storage
  - By default, logs are retained forever. You can configure log retention policies (from 1 day up to 10 years).
  - Storage - Standard log storage, Infrequent Access (Logs-IA) for lower cost
- Logs are encrypted by default. Optionally use Customer managed KMS key for logs encryption (for compliance and audit purpose). CloudWatch Logs


## Amazon CloudWatch Logs - Destination
*(Slide 1002)*

export
- Logs can be further sent to different AWS services for                                   Amazon S3 processing, storage and analytics.
- CloudWatch has built-in integration with
  - Amazon S3 (log exports and long-term storage)                                        Kinesis Data
  - Kinesis Data Streams                                                                    Stream
  - Amazon Data Firehose
streaming
  - AWS Lambda                                                                           Amazon Data Firehose CloudWatch Logs
AWS Lambda


## EC2 Logs
*(Slide 1003)*

- By default, EC2 logs are NOT sent to CloudWatch Logs. IAM Role
- We must install the CloudWatch Agent on the EC2 instance or on-premise server.
- Additionally, attach an IAM role with permissions to push logs to CloudWatch.    EC2 CloudWatch {                                     IAM Policy                                            Agent "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": [ "logs:CreateLogGroup", "logs:CreateLogStream", "logs:PutLogEvents", "logs:DescribeLogStreams" ], "Resource": "*" } ]                                                                          CloudWatch Logs }


## EC2 Logs
*(Slide 1004)*

- By default, EC2 logs are NOT sent to CloudWatch Logs.                                              IAM Role
- We must install the CloudWatch Agent on the EC2 instance or on- premise server.                                                                        EC2 CloudWatch
- Additionally, attach an IAM role with permissions to push logs to                                     Agent CloudWatch.
- CloudWatch Agents allows collecting extra metrics beyond default EC2 monitoring
  - CPU (granular): active, idle, user, system, guest, steal
  - Disk: space (free/used/total), I/O (reads, writes, bytes, IOPS)
  - Memory (RAM ): free, used, total, cached, inactive (EC2 default has NO RAM)
  - Network: TCP/UDP connections, packets, bytes
  - Processes: total, running, sleeping, blocked, dead, idle
  - Swap: free, used, used %
CloudWatch Logs


## Exercise - CloudWatch Logs
*(Slide 1005)*

```text
                                                                  1   Create an IAM role for EC2 with IAM policy
                                                                      CloudWatchAgentServerPolicy

                                                                  2   Attach this role to an EC2 instance

         IAM Role            Agent                                    Login to EC2 over SSH and install AWS CloudWatch agent
                                                                  3

                                                                  4   Create CloudWatch agent configuration file for setting up logs
                                                                      that you want to send to CloudWatch service
                                              Logs
                                                                  5   Start CloudWatch agent service

            EC2 Instance                             CloudWatch   6   Create a dummy log file in the directory as configured in the
                                                                      agent config file
                                                                  7   Go to CloudWatch Logs. You should see a new CloudWatch
                                                                      Logs group and Log stream. Check if you see the log entries.

                                                                  8   (Optional) Go to CloudWatch Logs insights and run few
                                                                      queries to filter the logs by some keywords e.g. ERROR

                                                                  9   After the exercise, terminate EC2 instance and delete
                                                                      CloudWatch Logs group
```


## Installing CloudWatch agent
*(Slide 1006)*


## Exercise - Useful commands
*(Slide 1007)*

1. Perform following action using root user. Run this command to change user to root: sudo su
2. Install AWS CloudWatch agent: yum install amazon-cloudwatch-agent
3. Create CloudWatch agent configuration file using vi or vim editor. Sample file config.json provided for download.
4. Start CloudWatch agent: /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch- config -m ec2 -c file:<path to the /config file that you created> -s
5. Check the status of CloudWatch agent: /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch- agent-ctl -m ec2 -a status
6. Now create dummy log file in the /var/log/ directory. Sample application.log provided for download.


## Exercise - Sample files
*(Slide 1008)*

1. config.json                                                                             2. application.log
{                                                                    INFO MainApp - Application started successfully. "logs": {                                                        DEBUG ServiceA - Initializing Service A with config: {timeout: 5000ms, retries: 3} INFO ServiceB - Fetching data from external API... "logs_collected": { ERROR ServiceB - Failed to fetch data from API: TimeoutException "files": {                                                   DEBUG DatabaseConnection - Attempting to connect to database at 10.0.0.1:5432 "collect_list": [                                          INFO DatabaseConnection - Connection to database established successfully. {                                                        ERROR UserService - User not found for ID: 12345 "file_path": "/var/log/application.log",               INFO Authentication - User login successful for username: johndoe "log_group_name": "/myapplication",                    DEBUG ServiceA - Processing user input: {userId: 12345, action: 'login'} INFO ServiceA - User session initialized for userID: 12345 "log_stream_name": "{instance_id}/application_logs", ERROR PaymentService - Payment failed: Insufficient funds for userID: 12345 "timestamp_format": "%b %d %H:%M:%S"                   DEBUG PaymentService - Payment request payload: {amount: 100.00, currency: }                                                        'USD'} ]                                                          INFO NotificationService - Sending email notification to userID: 12345 }                                                            ERROR NotificationService - Failed to send email to userID: 12345: }                                                              SMTPServerException DEBUG CacheManager - Cache miss for key: userDetails_12345 } INFO ServiceC - Scheduled job 'DataSync' completed successfully. }                                                                    DEBUG ServiceC - DataSync job processed 150 records. ERROR ServiceC - DataSync job failed: NullPointerException encountered in processRecords() INFO MainApp - Shutting down application... DEBUG MainApp - Application shutdown completed. Cleanup performed.


## CloudWatch additional Features – good to know
*(Slide 1009)*

- S3 Export
- CloudWatch Logs Subscription
- CloudWatch Logs Aggregation
- CloudWatch Logs Insights
- Additional Logs Insights:
  - Container Insights
  - Lambda Insights
  - Database Insights
  - Contributor Insights
  - Application Insights
- CloudWatch Dashboards and sharing


## CloudWatch Logs – S3 Export
*(Slide 1010)*

- Export CloudWatch Logs to Amazon S3 buckets
- Logs encryption using SSE-KMS and SSE-CMK
- Log can take up to 12 hours to be exported into S3 bucket (not real- time)
- Can use with S3 features like S3 Object Lock and configure retention period to keep the logs for long duration and to meet                     export regulatory or compliance requirements.
- Use cases: CloudWatch            S3 Bucket
  - Long term archival (cost benefits)
  - Logs query and analysis using AWS services like Athena
Tip: It’s cheaper to keep logs in S3 than CloudWatch


## CloudWatch Logs Subscription
*(Slide 1011)*

- Streams log events in real-time from CloudWatch                            Source                   Destination Logs to other AWS services for processing and analysis.                                                               AWS account                   AWS account
- Uses Subscription Filter to configure: Lambda
    - Which log events to capture (using filter patterns)
    - Where to send them (destination service) Cross-account IAM role
- Filters are applied at Log group level e.g.                                                                  Kinesis Data /aws/ecs/webapp/ (Max 2 subscriptions per group)                                                                Stream
- Supports cross-account subscription and delivery                    CloudWatch      Subscription   Resource-based IAM policy Logs            Filter                   Amazon Data
  - For Amazon Data Firehose target, configure IAM resource policy for Data Firehose to allow access to                                                     Firehose Account A CloudWatch service (logs.amazonaws.com)
  - For Kinesis Data Stream target, create Cross-account        { IAM role in Account B to allow access to Account A              "level": "ERROR", "service": "payment", CloudWatch service (logs.amazonaws.com)                                                       {$.level = "ERROR"} "message": "Credit
  - Lambda target is not supported for cross-account                  exceeded"                          Filter } Sample Logs


## CloudWatch Logs Aggregation
*(Slide 1012)*

- Supports aggregating logs across Multiple AWS account and multiple AWS regions
Account A Region 1
Destination CloudWatch Logs         Subscription Filter                          Account D
Account B Region 2 Amazon S3
CloudWatch Logs         Subscription Filter                         Kinesis Data   Amazon Data Stream       Firehose Account C Amazon Region 3                                        OpenSearch
CloudWatch Logs         Subscription Filter


## CloudWatch Logs Insights
*(Slide 1013)*

- Allows users to run SQL-like queries on CloudWatch       fields @timestamp, @message | filter @message like /ERROR/ Logs. | sort @timestamp desc
- Logs are analyzed in place without exporting or moving   | limit 20 data.
- Queries can be executed across multiple log groups simultaneously.
- The service supports filtering, parsing, sorting, and aggregation operations.
- Results can be visualized using charts or tables.
- Logs Insights is read-only and does not alter log data
- Use case: For ad-hoc investigation and debugging


## CloudWatch Insights – good to know
*(Slide 1014)*

CloudWatch Insights use logs and metrics together to provide service-specific visibility into application, compute, container, and database workloads.
- Container Insights: Monitors ECS, EKS, Kubernetes for container CPU, memory, network, pod & node metrics.
- Lambda Insights: Deep visibility into Lambda performance like memory usage, cold starts, duration, concurrency.
- Application Insights: Automatically detects application issues (latency, errors) using logs & metrics. Best for EC2 + EBS + ELB based apps.
- Database Insights: Performance monitoring for RDS & Aurora like load, waits, SQL performance.
- Contributor Insights: Identifies top contributors in logs/metrics. Example: Which IP/user/API causes most errors?


## CloudWatch Dashboards and sharing
*(Slide 1015)*

- Amazon CloudWatch offers automatic pre-built dashboards
- You can create your own custom dashboards to monitor your resources in a single view.
- Resources can be across different Regions and accounts.
- CloudWatch Dashboards can be shared with specific users via email, where users log in using a generated username and password.
- Dashboards can also be shared publicly using a URL, allowing anyone with the link to view them without AWS access.
- All shared dashboards are read-only and sharing can be revoked at any time.


## AWS X-Ray
*(Slide 1016)*


## Microservices application in real world
*(Slide 1017)*

```text
    Service
    teams


                                                                      Order
                                                                     Service

                                              Backend                                                       Database
                                              Service




                                                                                        Billing &
                                                                                        Payment
                                                   Queue                                                               CRM
               UI Service                          Service




                                                                               Search
                                                             Email
                                                                                                      Product
                                                                                                      Catalog
                    You need tool to capture the traces of the request as it goes from one service to the other..
```


## AWS X-Ray
*(Slide 1018)*

- Service that collects data for requests that your application serves.
- Provides tools that you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization.
X-Ray Daemon                                                      X-Ray Console
X-Ray SDK
AWS X-Ray
.Net                                                           Web Browser Java           Node.js
Application Code


## AWS X –Ray : Visual Analysis of Applications
*(Slide 1019)*

```text
                                                                            Session Database
                                                                          AWS::DynamoDB::Table




                                                  Application                                 Application state
                         Client               AWS::ECS::Container                           AWS::DynamoDB::Table




                                                                     Application Database               Notification
                                                                    AWS::DynamoDB::Table                AWS::SNS
```


## Use AWS X-Ray for..
*(Slide 1020)*

- Debugging slow responses to the user requests
- Troubleshooting increased error rates
- Tracing down the user request path -> Traces
- Are requests getting completed in SLA time?                    AWS X-Ray
- Which service is the bottleneck in the application?
- Which users are impacted by the specific service disruption?
- More..


## AWS Health
*(Slide 1021)*


## AWS Health Dashboard - Service health
*(Slide 1022)*

- Shows AWS services disruption events


## AWS Health Dashboard - Service history
*(Slide 1023)*

- Shows AWS services disruption events
- Shows current and historical status of the AWS services health


## AWS Health Dashboard – Your account
*(Slide 1024)*

- Provides alerts and remediation guidance when AWS is experiencing events that may impact you.
- Proactive notification to help you plan for scheduled activities.


## AWS Health integration with EventBridge
*(Slide 1025)*

- AWS Health automatically publishes service health events to Amazon EventBridge.
- EventBridge rules can filter events based on service, region, or event type.
- Matching events can trigger automated actions such as:
  - Send SNS notifications
  - Trigger Lambda functions
  - Run SSM Automation runbooks
  - Send message to SQS queue, Kinesis data stream
  - Or create tickets in 3rd party systems like JIRA, ServiceNow                 Amazon AWS Health                 Rule
- This enables proactive response to AWS outages or                                     EventBridge maintenance affecting your account.


## AWS Systems Manager (SSM)
*(Slide 1026)*

*AWS CloudTrail*

*AWS Config*


## Managing AWS resources
*(Slide 1027)*

```text
                                              Manage EC2 instances
                                              - Apply security patches
                                              - Monitor processes, filesystems,
                                                 users
                                              - Run remote commands
                                              - Connect                AWS Systems Manager
```


## AWS Account Activity - Tracking
*(Slide 1028)*

```text
                                              Who did what in your AWS account

                                              -   Who launched EC2 instance and
                                                  when
                                              -   Who logged into AWS console
                                              -   Who deleted S3 bucket
                                              -   Who create DB snapshot     AWS CloudTrail
                                              …
```


## AWS Resource configuration and state - Tracking
*(Slide 1029)*

```text
                                              Track the state of AWS resources
                                              - Are all security group rules ok?
                                              - Is there any S3 bucket publicly
                                                 accessible?
                                              - Is backups enabled?
                                              - Are IAM access keys rotated?
                                              - Are SSL Certificates expiring?
                                                                                 AWS Config
                                              - Is autoscaling configured with multi-
                                                 az?
                                              - Is RDS multi-az?
                                              - Is CloudTrail enabled?
```


# AWS Systems Manager and More

*(Source: Slide 1030)*

### AWS Systems Manager


## Managing servers
*(Slide 1031)*

```text
              On-premises                                                      AWS
                                              ✓   Apply security patches
                                              ✓   Run remote commands
                                              ✓   Connect
                                              ✓   Install/Uninstall software
                                                                                 EC2


                         )

                                                        Admin                        EC2




                 Servers
                                                                                     EC2
```


## AWS Systems Manager (SSM)
*(Slide 1032)*

- Automates common and repetitive IT operations and management tasks                   EC2 or Server
- Works for Linux, Windows, MacOS, and Raspberry Pi OS
- Run Command - Author and execute runbooks (SSM documents) across fleet of EC2 or on-premises servers
- Session Manager – Provides secure access over HTTPS to connect to SSM Agent instances and servers managed by Systems Manager
- Fleet Manager - Get operational insights about the state of your infrastructure
- Patch Manager - Select and install OS and software security patches                 send      report actions    status
- State Manager – Define the desired configuration (e.g. agents installed) for EC2 instances/servers and apply/re-apply the state by running SSM documents
- Parameter Store - Store and manage configuration data and credentials centrally
- More features.. AWS Systems Manager (SSM)


## Systems Manager - Session Manager
*(Slide 1033)*

- Allows you to start a secure remote session to your EC2 instances or any virtual machine having SSM agent installed.
- Supports Linux, macOS, and Windows.
- No bastion hosts or SSH keys or SSH port (22) needed.
- Access is granted through AWS IAM role/policies.
o Agent c nnects to SSM IAM Permissions
Connect                        Connect
User                                  Session Manager                           EC2 Instance (SSM Agent)


## How it works?
*(Slide 1034)*

```text
        The managed nodes you connect to must also allow HTTPS (port 443) outbound traffic to the following
        endpoints:
        ✓ ec2messages.region.amazonaws.com
        ✓ ssm.region.amazonaws.com
        ✓ ssmmessages.region.amazonaws.comInstance
                                                                                                                      EC2 in Public subnet


                                                                                                 Public subnet        Private subnet


                                                              Session starts


     User                                                                Agent connects to SSM
                          Session
                                                                                                       EC2 Instance
                          Manager
                                                                                                       (SSM Agent)
                   Endpoints:
                   ec2messages.region.amazonaws.com
                   ssm.region.amazonaws.com
                   ssmmessages.region.amazonaws.comInstance
```


## How it works?
*(Slide 1035)*

```text
        The managed nodes you connect to must also allow HTTPS (port 443) outbound traffic to the following
        endpoints:
        ✓ ec2messages.region.amazonaws.com
        ✓ ssm.region.amazonaws.com
        ✓ ssmmessages.region.amazonaws.comInstance
                                                                                                                  EC2 in Private subnet


                                                                                              Public subnet         Private subnet


                                                              Session starts


     User                                                                      Agent connects to SSM
                          Session                                                                                        EC2 Instance
                          Manager                                                                   NAT gateway          (SSM Agent)
                   Endpoints:
                   ec2messages.region.amazonaws.com
                   ssm.region.amazonaws.com
                   ssmmessages.region.amazonaws.comInstance
```


## How it works?
*(Slide 1036)*

```text
        The managed nodes you connect to must also allow HTTPS (port 443) outbound traffic to the following
        endpoints:
        ✓ ec2messages.region.amazonaws.com
        ✓ ssm.region.amazonaws.com
        ✓ ssmmessages.region.amazonaws.comInstance
                                                                    EC2 in Private subnet without outbound
                                                                    internet access


                                                                  Public subnet          Private subnet




     User
                          Session                                                             EC2 Instance
                          Manager                                       NAT gateway           (SSM Agent)
                   Endpoints:
                   ec2messages.region.amazonaws.com
                   ssm.region.amazonaws.com
                   ssmmessages.region.amazonaws.comInstance
```


## How it works?
*(Slide 1037)*

```text
        The managed nodes you connect to must also allow HTTPS (port 443) outbound traffic to the following
        endpoints:
        ✓ ec2messages.region.amazonaws.com
        ✓ ssm.region.amazonaws.com
        ✓ ssmmessages.region.amazonaws.comInstance
                                                                                 EC2 in Private subnet without outbound
                                                                                 internet access


                                                                               Public subnet         Private subnet


                                                              Session starts


     User
                          Session                   Agent connects to SSM                                 EC2 Instance
                          Manager                                                                         (SSM Agent)
                   Endpoints:
                   ec2messages.region.amazonaws.com
                   ssm.region.amazonaws.com
                   ssmmessages.region.amazonaws.comInstance
```


## Exercise – Systems manager for connecting and managing
*(Slide 1038)*

EC2 instances 1   Create IAM Role for EC2 with permissions AmazonEC2RoleforSSM Launch EC2 instance (Amazon Linux) and 2 o                                      associate this IAM role while launching. Agent c nnects to SSM                         • Amazon Linux comes with SSM agent pre- installed
- For security group - No need to have any Start session                                  inbound rule. Run command 3   Go to AWS Systems Manager and Session Fleet Manager EC2 Instance                                                                    Manager -> Start Session with EC2 instance AWS Systems Manager (SSM Agent) 4   Go to Systems Manger -> Fleet Manager and explore options to manage EC2 instance
Go to Systems Manager -> Run command and 5 execute simple Shell command on EC2
6   Go to EC2 console -> Select Instance -> Connect -> Session Manager 7   After the exercise, terminate EC2 instance


## Troubleshooting – If instance does not appear in Systems
*(Slide 1039)*

```text
      Manager
         1    Check if Systems Manager agent is running on EC2 instance
              a) Open SSH port and connect to instance from your local workstation
              b) Check if systems manager agent is running by running the command:
              $sudo systemctl status amazon-ssm-agent

         2    Check if correct IAM role is associated with EC2 instance
              a) For EC2 instance check the IAM role name associated with the instance
              b) Go to IAM role and verify the IAM policy attached to the role. It should be: AmazonEC2RoleforSSM


         3    Check if EC2 security group allows Outbound traffic
              a) For EC2 instance to connect to Systems Manager endpoint, it needs outbound HTTPS connection
              b) Check if EC2 security group allows outbound traffic (by default all outbound traffic is allowed)


         4    Restart SSM agent on EC2 instance
              a) If you modified the IAM role after EC2 instance is launched, you may have to restart the SSM agent.
              b) Connect to EC2 instance over SSH from your workstation and run the command:
              $sudo systemctl restart amazon-ssm-agent
```


## SSM Documents
*(Slide 1040)*

- SSM documents define what actions AWS Systems         Manual Manager should perform on managed resources.
- SSM documents are written in JSON or YAML and are versioned. Maintenance
- They can be AWS-managed or customer-created.         Windows
- SSM documents are used by Run Command, Automation, State Manager, Patch Manager, and Session Manager. Scheduled
- Sample documents:                                                     SSM      Run
  - AWS-RunShellScript                                        Document Command
  - AWS-RunPowerShellScript
  - AWS-UpdateSSMAgent                        State Manager
  - AWS-ConfigureAWSPackage
  - AWSEC2-PatchLoadBalancerInstance
EventBridge


## SSM Automation document
*(Slide 1041)*

- Automation documents define step-by-step workflows for operational     Manual tasks.
- They support branching, waiting, approvals, and rollback.
- Automations can be triggered manually, on a schedule, or by          Maintenance Windows maintenance window or via State Manager or EventBridge.
- Common uses include patching, remediation, maintenance, and deployments.
- Automation documents run with an assumed IAM role.                    Scheduled SSM
- Example: AWSEC2-PatchLoadBalancerInstance Automation
  - Deregisters an EC2 instance from the load balancer                         Document
  - Applies OS patches to the instance State Manager
  - Reboots the instance if required
  - Re-registers the instance back to the load balancer
EventBridge


## SSM Parameter Store
*(Slide 1042)*

Development                                  Production
- It stores key-value parameters such as configuration values, URLs, and credentials.
- Parameters can be plain text or encrypted using AWS KMS.          Lambda function                             Lambda function
- Access to parameters is controlled using IAM policies.
/AppA/serviceA/envi/dev/logLevel/
/AppA/serviceA/envi/prod/logLevel/
- Applications and services can retrieve parameters securely at runtime.
- Supports versioning and parameter history.
- Stores parameters in hierarchical format
  - /AppA/serviceA/env/dev/logLevel = INFO
  - /apiBaseURL = api-dev.example.com
  - /requestTimeout = 1000ms
  - /AppA/serviceA/env/prod/logLevel = ERROR
  - /apiBaseURL = api.example.com
  - /requestTimeout = 100ms
INFO                                       ERROR


## AWS CloudTrail
*(Slide 1044)*


## AWS CloudTrail
*(Slide 1045)*

AWS account
- AWS CloudTrail is a service that records everything that happens in your AWS account.​                                                                      CloudTrail
- You can see who did what, when, from where, and using which service.​
- Records event history of API calls and user actions made through:                                Event
  - AWS Management Console / SDK / CLI / Other AWS services                                      History
- By default, CloudTrail shows last 90 days of management events in Event History.
- CloudTrail logs can be delivered centrally using Trail. Destination can be:
    - Amazon S3 (for long-term storage) ?             Trail
    - CloudWatch Logs (for monitoring and alerts)
    - Amazon EventBridge
- Trail can be enabled for All regions (Multi-region) or a single region.
Amazon S3


## CloudTrail - Ways to store and access events
*(Slide 1046)*

```text
        AWS
     Management
      Console                                                                                      Trail
                                                                                                                      CloudWatch
                                              AWS APIs                                                                   Logs
               CLI

                                                                    AWS CloudTrail
                                                                                                   Trail
               SDK                                           Inspect & Audit
                                                                 events                                       S3 Bucket
                                                                                   Query using Athena

                                                                                   Query using CloudWatch Logs Insights
                                                   Event
                                                   History               Auditor
```


## AWS CloudTrail – good to know
*(Slide 1047)*

- CloudTrail Event types
  - Management Events: Records control-plane actions like creating, modifying, or deleting AWS resources. Example: RunInstances, CreateBucket etc.
  - Data Events: Records data-plane actions on resources such as S3 objects or Lambda functions. Example: GetObject, PutObject (not enabled by default)
- CloudTrail Insights
  - CloudTrail Insights detects unusual API activity using ML.
  - Helps identify spikes, anomalies, or suspicious behavior.
  - Works on management events only.
  - Example: Sudden increase in TerminateInstances API calls.
- CloudTrail Lake
  - CloudTrail Lake provides a fully managed immutable event store and allows SQL-based queries
  - Events are aggregated into event data stores, which are immutable collections of events based on criteria you define e.g. Management events, Data Events, Insights events etc.
  - Converts JSON format events to Apache ORC format for easy and efficient query
  - Supports long-term retention (7 years and 10 years retention options)


## AWS Config
*(Slide 1048)*


## AWS Config
*(Slide 1049)*

- AWS Config is a service that tracks and records the configuration state of AWS resources over time.
- It helps answer what resources exist, how they are configured, and how they have changed.
- Configuration changes can be sent as a notification and be stored in S3 bucket as a snapshot.
- Config Rules define desired configuration conditions and evaluate resources as compliant or non-compliant.
- Actions include notifications via SNS and automatic or manual remediation using Systems Manager Automation documents.
Compliant
Record
AWS                X AWS Config   Config rules         X Non-compliant AWS resources                                                            X


## AWS Config
*(Slide 1050)*

Questions that can be answered by AWS Config:
- Is there a security group which has port 22 (SSH) open for the world (0.0.0.0/0)?
- Are there expired ACM certificates?
- Are there any resources which are not tagged?
- Are there any S3 buckets in my account which are Public (can be read by anyone)?
- Is there any DynamoDB table having provisioned capacity above 100 WCU?
- Is there a RDS database which is not multi-AZ?


## AWS Config – Features
*(Slide 1051)*

- Aggregators: Configuration Aggregators collect and provide a centralized view of resources across accounts and regions.
- Advanced Queries: You can use AWS Config to query the current configuration state of AWS resources based on configuration properties for a single account and Region or across multiple accounts and Regions.
SELECT * WHERE resourceType = 'AWS::EC2::SecurityGroup’ AND configuration.ipPermissions.ipRanges BETWEEN '10.0.0.0' AND '10.0.0.255’ AND NOT configuration.ipPermissions.ipRanges < '10.0.0.0’ AND NOT configuration.ipPermissions.ipRanges > '10.0.0.255'
- Remediation: AWS Config allows you to remediate noncompliant resources using AWS Systems Manager Automation documents
- Conformance pack: A conformance pack is a collection of AWS Config rules and remediation actions that can be easily deployed as a single entity in an account and a Region or across an organization in AWS Organizations.


## AWS Config – Features
*(Slide 1052)*

```text
                                                                                     Notification
                                                                                                           SNS
                                                                    Configuration
                                                                      changes           Deliver
                                                                                        snapshot            S3
                                              Record                                                               Send findings
                                                                                                                                   Security Hub CSPM
                                                                                       Rule

                                                       AWS Config                                                  Remediation
                 AWS resources                                                                                                     Systems Manager
                                                                                                 Config rules
                                                                                                                                    Automation Document


                                                                                    Aggregator                                     Centralized
                                                                                                                                   dashboard


                                                                                                                                   Advanced
                                                                                                 Aggregated data
                                                                                                 across accounts                    Queries
                                                                                                   and regions
```


## AWS Config - Features
*(Slide 1053)*

Remediation User
- AWS Config allows you to remediate noncompliant resources that are evaluated by AWS Config Rules.
- Remediation actions are executed using AWS Systems Manager Automation documents.
- Remediation can be configured to retry automatically if it fails and you can specify the maximum retry attempts and retry interval.
- Systems Manager execution role needs to have permissions                              Rule AWS Config                   S3 Bucket to perform the remediation actions.
SSM Systems Manager   Automation Document


# AWS Security Services

*(Source: Slide 1054)*


## AWS Security services
*(Slide 1055)*

```text
                                                                             Network and        Threat Detection and
      Identity and access                           Data protection
                                                                            Infrastructure     incidence management

                AWS IAM                       AWS Key Management        VPC Security Groups     Amazon GuardDuty
                                                 Service (KMS)              and NACL

          AWS IAM Identity                                                                      Amazon Inspector
              Center                          AWS Certificate Manager        AWS WAF

                                                                                                 Amazon Detective
           Amazon Cognito                      AWS Secrets Manager          AWS Shield

                                                                                                 AWS Security Hub
                                                  Amazon Macie          AWS Firewall Manager


                                                                        AWS Network Firewall
```


## Web Application layer
*(Slide 1056)*

```text
   security (HTTP/HTTPs)

                                     WAF

     Distributed Denial of
       Service (DDoS)
           Protection
                                   Shield



    Vulnerability scanning
                                                                  Aggregate security
                                                                 incident data across
                                 Inspector                         various sources

                                                  Security Hub
        Intelligent Threat
            Detection

                                 GuardDuty


   Investigate and analyze
   incidents using ML and
      find the root cause                     Amazon EventBridge
                                  Detective
```


## AWS WAF
*(Slide 1057)*


## AWS Web Application Firewall (WAF)
*(Slide 1058)*

```text
                                              AWS WAF




                          X                             CloudFront    Application API Gateway   AppSync
                                                                     Load Balancer


                          X
```


## AWS WAF – Web Application Firewall
*(Slide 1059)*

- Protects web applications from common web exploits (Layer 7) – OWASP Top 10
- Deploy on Application Load Balancer, API Gateway or AppSync GraphQL APIs and CloudFront
- Define Web ACL (Web Access Control List):
  - Block IP addresses
  - Filter traffic based on HTTP headers, HTTP body, or URI strings
  - Detect and block common attacks - SQL injection and Cross-Site Scripting (XSS)
  - Geo-match – allow or block countries or regions
  - Rate-based rules (to count occurrences of events) – for DDoS protection
- Can present CAPTCHA or challenge to prevent bot attacks
- On blocking the malicious traffic WAF returns HTTP 403 status code (Forbidden)


## AWS Shield
*(Slide 1060)*


## What’s a DDOS attack?
*(Slide 1061)*

```text
         Distributed Denial-of-Service
                                                               Regular users


                                                                         Not accessible
                                                                         Not responsive




                  Attacker


                                                                   Application
                                                                     server




                                              Primary


                                                        Bots
```


## Common DDoS attacks
*(Slide 1062)*

- SYN Flood attack: Too many half open TCP connections
- UDP Flood attack: Too many UDP requests
- UDP Reflection attack: Spoof the victim server IP as a source for UDP packet. Victim server receives the unexpected responses.
- DNS Flood attack: Overwhelm the DNS so legitimate users can’t find the site
- Slow Loris attack (Layer7): A lot of HTTP connections are opened and maintained
- Cache Busting attack: Request un-cached data from CDN


## AWS Shield
*(Slide 1063)*

```text
           Provides protection against DDoS attack at Network Layer (layer3) and Transport Layer (layer4)




                                               AWS
                                               Shield
                                                                   CloudFront    Application    API Gateway
                                                         AWS WAF
                                                                                Load Balancer



                          X                    Shield
                                              Advanced
```


## AWS Shield
*(Slide 1064)*

AWS Shield Standard:
- Free service that is activated for every AWS customer
AWS Shield Advanced:
- Optional DDoS mitigation service ($3,000 per month per organization)
- Protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53
- 24/7 access to AWS Shield Response Team (SRT)
- Protect against higher fees during usage spikes due to DDoS


## Reference Architecture for DDoS Protection
*(Slide 1065)*

```text
                                                                    VPC



                                                                       Public subnet   Private subnet

                                        AWS Shield   AWS Shield


                                                                          AWS Shield



                                                     CloudFront                              EC2
         Users                           Route 53
                                                     Distribution          Load          Auto Scaling
                                                                          Balancer          Group

                                                                      Security group   Security group




                                                     AWS WAF
```


## AWS Network Firewall
*(Slide 1066)*


## AWS Network Firewall
*(Slide 1067)*

```text
        A stateful network firewall and intrusion detection and prevention service for VPC




                                                                   Internet Gateway


                                              VPC

                                                Network ACL


                                                              SG


                                                                                      Firewall   AWS Network
                                                                                      endpoint     Firewall
                                                    Subnet
```


## AWS Firewall Manager
*(Slide 1068)*


## AWS Firewall Manager
*(Slide 1069)*

- Centrally configure and manager Firewall rules across AWS accounts
- Manages the rules for AWS WAF, AWS Shield Advanced, Amazon VPC security groups, AWS Network Firewall, and Amazon Route 53 Resolver DNS Firewall
- Integrates with AWS Organization:
  - Provides centralized monitoring of DDoS attacks across AWS AWS Firewall organization Manager
  - New accounts added under the AWS Organizations are automatically protected


## Amazon Inspector
*(Slide 1070)*


## Amazon Inspector
*(Slide 1071)*

- Amazon Inspector is a vulnerability management service that continuously monitors your AWS workloads for software vulnerabilities and unintended network exposure.
- Scans EC2 instances, Container Images (ECR) & Lambda functions for software vulnerabilities (CVEs)
- Publishes findings to Amazon EventBridge and AWS Security Hub for reporting and action
- Inspector can be centrally managed across multiple accounts using AWS Organizations.
SSM Agent Installed s/w packages exposed to Common Vulnerabilities and Exposures (CVEs) EC2 Security Hub Network exposures – Open ports (TCP/UDP)
ECR            Amazon Inspector Code vulnerability – injection flaws, data leaks, sensitive                                                       EventBridge data, missing encryption etc. CVEs Lambda


## Amazon GuardDuty
*(Slide 1072)*

- Intelligent threat detection service - Enable with just “One click”
- Uses Machine learning to detect threats more accurately
- Can detect sophisticated attacks e.g. EC2 instance being used for crypto currency or Bitcoin mining, data exfiltration, unusual access to malicious IP etc.
- Analyses tens of billions of events across multiple AWS data sources such as:
  - AWS CloudTrail logs: Unusual API calls, unauthorized deployments.
  - VPC Flow Logs: Unusual internal traffic, unusual IP address.                         Amazon GuardDuty
  - DNS query logs: Compromised EC2 instances sending encoded data within DNS queries.
- Notifies the security findings:
  - Amazon EventBridge
  - AWS Security Hub


## Amazon GuardDuty
*(Slide 1073)*

```text
                                                                  Findings    Actions

                                                  GuardDuty



             VPC flow logs                         Threat                    Security hub
                                                intelligence



               DNS Logs
                                                                             Event Bridge
                                              Anomaly Detection
                                                   (ML)
                                                                                SIEM/Partner
              CloudTrail                                                          Solutions
               Events
```


## Amazon GuardDuty
*(Slide 1074)*

```text
                                                                                75+ threat and anomaly
                                                                                   detection checks

                                                  GuardDuty
                                                                  EC2: C&C, DoS, Spambot,
                                                                  EC2: Unusual Network Port, Traffic Volume
                                                                  EC2: Bitcoin, Port scan, RDP/SSH Brute force
             VPC flow logs                         Threat
                                                intelligence
                                                                  EC2: C&C DNS, Bitcoin DNS
                                                                  EC2: Abused domain, Phishing domain

               DNS Logs


                                              Anomaly Detection   IAM: Anomalous Behavior (GetPassword etc.)
                                                   (ML)           IAM: Kali/Parrot/Pentoo Linux
                                                                  IAM: Root Credential usage,
              CloudTrail                                          S3: Malicious IP caller, Tor IP caller, Unusual object read,
               Events                                             S3: Bucket anonymous access granted, Block public access
                                                                  disabled etc.
                                                                  EC2:Cryptocurrency/Bitcoin
```


## AWS Security Hub
*(Slide 1075)*


## AWS Security Hub
*(Slide 1076)*

- Centrally collects security data from across AWS accounts and services.
- Helps analyze security trends to identify and prioritize the security issues across your AWS landscape.
- Automatically aggregates alerts from various AWS services and AWS partner tools such as:
  - Amazon GuardDuty
  - Amazon Inspector
  - Amazon Macie                           Must enable AWS Config service so that AWS resource state can be captured and analysed
  - IAM Access Analyzer
  - AWS Systems Manager
  - AWS Firewall Manager
  - AWS Partner Network Solutions


## AWS Security Hub
*(Slide 1077)*

```text
                                                                               Other AWS
                                                                                Accounts

                                                                                                          EventBridge
                                                                                                             Event

             Systems                IAM Access   Firewall    Collects Issues                  Automated
             Manager                  Analyser   Manager      and Findings                     Checks


                                                                                                          Security Hub
                                                                               Security Hub                 Findings

             Amazon                     Amazon    Amazon
            GuardDuty                    Macie   Inspector
                                                                               Third-Party
                                                                                Partners
                                                                                                      Amazon Detective
```


## Amazon Detective
*(Slide 1078)*

- Uses Machine learning and Graph technology to analyze, investigate, and identify the root cause of security issues
- Uses network traffic data, AWS account activity events and security findings from Amazon Macie, Amazon GuardDuty, Security Hub etc.


# Storage and Data Migration

*(Source: Slide 1079)*

### Other Storage services, Hybrid Storage and

*Data Migration services*


## AWS Storage services
*(Slide 1080)*

```text
                       Block                                                     File                                       Object




                            EC2               Amazon             Amazon    Amazon FSx Amazon FSx Amazon FSx                 Amazon S3
               Amazon                                            FSx for
                          instance             EFS                         for Windows  for Lustre for OpenZFS
                EBS                                              NetApp
                           Storage                                          File Server
                                                                 ONTAP




                                              Hybrid Storage and Data Transfer                                                   Backup
      Hybrid Storage




                                                 Data Transfer




                         AWS Storage                                         AWS         AWS Transfer   AWS Data Transfer
                          Gateway                                          DataSync                        Terminal             AWS Backup
                                                                                           Family
```


## Elastic File System (EFS)
*(Slide 1081)*


## AWS Storage services
*(Slide 1082)*

```text
                                                                   Availability Zone 1      Availability Zone 2     Availability Zone 3




                                                                              Containers   EC2        Containers
     Block Storage




                                                                   EC2                                             EC2



                               Elastic Block Store                Volume          Volume   Volume       Volume     Volume
                                      (EBS)
    File Storage




                     Elastic File                                        mount                   mount                   mount
                                    FSx for
                                                     Filesystem
                      System        Windows
    Object Storage




                           Simple Storage Service     Bucket
                                    (S3)
```


## Amazon Elastic File System (EFS)
*(Slide 1083)*

- A managed Network File System (NFS 4)
- Can be shared by hundreds of EC2 Linux instances                                                  EC2        ECS        EKS       Fargate Lambda SageMaker
- Works with most of the AWS compute services
- Serverless - no need to manage infrastructure              Availability Zone      Availability Zone   Availability Zone
- Elastic – no need to pre-provision the capacity, NFS                     NFS                   NFS pay as per the storage used                                         client                  client                client
- Use cases:
  - Containerized and serverless applications                 mount                   mount               mount
  - Machine learning training
  - Web serving and content management
  - User home directories File System
Elastic File System


## Amazon Elastic File System (EFS)
*(Slide 1084)*

- EFS filesystem can be accessed by Region instances in a different VPC within or across AWS regions over VPC peering connection and from On-premises host if            Availability Zone    Availability Zone it’s connected to the VPC over VPN or                                                                            NFS Direct connect                                                                                                   client NFS                  NFS client               client
VPN or Direct File System                     Connect On-premise NFS client EFS


## EFS Filesystem storage options – Regional vs OneZone
*(Slide 1085)*

Standard Storage (Regional)
- Stores data redundantly across multiple Availability Zone   Availability Zone   Availability Zone
Availability Zones in the region
- Provides 11 9’s of durability File System
OneZone Storage
- Stores data within in a single Availability Zone            Availability Zone   Availability Zone   Availability Zone
- Ideal for non-critical / reproducible data
- 50% cheaper than standard regional store filesystem                                    File System


## EFS Storage Classes – Based on access pattern
*(Slide 1086)*

EFS Standard
- SSD storage designed to deliver sub-millisecond latency for active data
Standard EFS Standard–Infrequent Access (EFS–IA)                                                                          30 days
- Cost-optimized for data accessed only a few times a quarter which doesn’t need the sub-millisecond latencies of EFS Standard.
- EFS Lifecycle policy “Transition into IA” automatically moves the files from Standard storage to Standard-IA if files are not accessed for 30 days Standard-IA 90 days EFS Archive
- Cost-optimized for long-lived data accessed a few times a year or less and offering similar performance to EFS IA
- EFS Lifecycle policy “TransitionToArchive” automatically moves the files to EFS archive storage if files are not accessed for 90 days
- EFS Lifecycle policy “Transition to Standard” is set to None by default. Archive


## Amazon FSx
*(Slide 1087)*


## Amazon FSx
*(Slide 1088)*

- Like-to-Like 3rd party fully managed file systems on AWS
Lustre / HPC      ZFS / Linux Windows NetApp              Parallel file    based File File Server                                       system            Server
FSx for Windows File             FSx for NetApp ONTAP   FSx for Lustre    FSx for OpenZFS Server


## FSx for Windows File Server
*(Slide 1089)*

- A fully managed, reliable and scalable Windows shared file system Availability Zone   Availability Zone
- Uses Windows File Server
- Supports Windows NTFS and SMB protocols
- Accessible across VPCs, across AWS regions and \\fs-0123456789.example.com\folder accounts
- Accessible from on-premises servers
- Integration with Active Directory                            FSx for             FSx for Windows File        Windows File Server              Server


## FSx for Lustre
*(Slide 1090)*

On-premises
- High performance File System for HPC workloads shared by thousands of compute machines
- Data stored in Amazon S3 is loaded to FSx for Region processing
- Output of data processing is sent to Amazon S3 for retention Link
- Use cases:
  - Access ML training data in S3                                    FSx for Lustre
  - Computational Fluid Dynamics (CFD) simulations Compute
  - VFX, Rendering, Transcoding S3


## AWS Storage services comparison - Cost
*(Slide 1091)*

< > S3                                         EBS                               EFS
- Pay as you go.
- Pay for provisioned
- Pay as you go.
- No need to pre-provision                        capacity
- No need to pre-provision capacity.                                  •    Pay even if not used entire       storage
- Per GB per month (e.g.                          space
- Per GB month (e.g. $0.3 per $0.023 per GB) + $0.005 per                •    Per GB per month (e.g.            GB) 1000 requests for S3                            $0.1 per GB) standard
Filesystem
*Prices in N. Virginia region at the time of recording this video


## AWS Storage services – match the pairs !
*(Slide 1092)*

```text
          Processing large scientific datasets by
                multiple compute nodes
                                                                       Volume
        Social Media Images accessible to users
                                                         EBS
        Social Media Images accessible to Linux
             EC2 instances for processing
                                                                       Filesystem
            Temporary high-throughput storage
                   integrated with S3                    EFS
              Storage for a relational database
                                                                       Filesystem
                      Hosting static website
                                                    FSx for Windows
                Storing backups and archives

         Shared storage for Microsoft workloads                        Filesystem

         Data lake storage for analytics and ML       FSx for Lustre
                       workloads

            Persistent filesystem for containers                         Bucket
            Hosting an OS for an EC2 instance
                                                          S3
```


## Migrating data to AWS
*(Slide 1093)*

```text
               App          App        App




                                                                            EC2



           Raw storage Files storage                                   S3   EFS   FSx




                                              AWS Database Migration
                       Databases                 Service (DMS)
                                                                            AWS
                      On-premises
```


## Migrating data to AWS
*(Slide 1094)*

```text
                                              Data
               Customer
               Datacenter
```


## How much time does it take to move data online?
*(Slide 1095)*

```text
                             Bandwidth ->
              Data size ->




                                              1 Gbps    2 Gbps    5 Gbps     10 Gbps

                                  500 TB      58 days   29 days   12 days     6 days

                                     5 PB     2 years   1 year    116 days   58 days

                                   10 PB      4 years   2 years   232 days   116 days
```


## Offline data transfer with AWS Snow devices
*(Slide 1096)*

```text
                                                                          Service Discontinued




               Customer
               Datacenter                       AWS        AWS Snowball
                                              Snowcone        Edge




                                                  Ship data to AWS
```


## Offline data transfer with AWS Data Transfer Terminal
*(Slide 1097)*

```text
                     Customer
                     Datacenter
                                                           Nearest location



                                                                              High-speed connection

                                              AWS Data Transfer
                                                 Terminal
```


## AWS Snowcone
*(Slide 1098)*

Service Discontinued Small, portable, rugged, and secure edge computing and data transfer device
- Encryption, tamper-evident
- 4.5 pounds (2.1 kg)
- Portable
- Withstands harsh environments
- 8 TB HDD or 14 TB SSD storage
- 2 CPUs, 4 GB of memory
- Wi-Fi or Ethernet data copy
- AWS DataSync agent pre-installed for online data transfer
Use cases Industrial IoT – sensor or machine data in a factory, Content distribution and aggregation, data migration


## AWS Snowball Edge
*(Slide 1099)*

Service Discontinued Snowball device with on-board storage and compute power
- 49.7 pounds (22.54 Kg)
- 2 x 10 Gbit, 1 x 40 Gbit, 1 x 100 G-bit Network Interfaces
- Device Options - Storage optimized / Compute optimized
- Up to 104 vCPU, 416GB RAM
- Up to 80TB HDD and 210TB SSD
Use cases
  - Storage optimized – Large scale data migration
  - Compute optimized – Edge (on-premises) data processing, machine learning, full motion video analytics,


## Online data transfer with AWS DataSync
*(Slide 1100)*

```text
                                              AWS DataSync




               Customer
               Datacenter
```


## AWS DataSync
*(Slide 1101)*

```text
                                       On premises


                                                                 DataSync
                                                                  agent


                            NFS, SMB            Hadoop (HDFS)

                                                                                       Amazon S3
                                              Edge




                                                                              AWS      Amazon FSx
                             AWS                 Amazon S3                  DataSync
                           Snowcone              on Outposts

                               Other Cloud Providers

                                                                                       Amazon EFS
                                                                DataSync
                                                                 agent
                                Google           Azure
                             Cloud Storage
```


## AWS DataSync
*(Slide 1102)*

- Moves data between on premises and AWS Storage services
- Supports NFS, SMB, HDFS storage and AWS storage services such as S3, EFS and FSx.
- Data transfer tasks can be scheduled – Hourly, daily, weekly etc.
- You can set limit on the bandwidth usage by DataSync. Single task can utilize up to 10 Gbps. AWS DataSync
- Data is copied incrementally and it handles failure during the transmission.
- Can run multiple tasks in parallel
Supports TLS encryption
/Folder A   Task 1                                    /Folder A
/Folder B   Task 2                                    /Folder B NAS storage system                      /Folder C   Task 3                                    /Folder C


## AWS Transfer Family
*(Slide 1103)*

IAM IDP
- A fully managed service for file transfer into and out of AWS over SFTP, FTPS, and FTP.
- Customers can retain existing file-transfer workflows by integrating their application with AWS Transfer family                                                                     Amazon S3 IAM Role
- Integrates with Amazon S3 and Amazon EFS for storage
- Supported protocols for the file transfer:
  - File Transfer Protocol (FTP)                                       FTP        AWS Transfer
  - File Transfer Protocol over SSL (FTPS)                             Users        Family
  - Secure File Transfer Protocol (SFTP)
  - Application Statement 2 (AS2) Amazon EFS
- Supports user authentication with IAM, Active Directory, LDAP and other 3rd-party identity providers.
- Use cases:                                                                         AWS FTP     AWS FTPS
  - Secure B2B file exchange with partners
  - EDI documents transfer
  - Replacing on-premises FTP/SFTP servers & integrating legacy apps
  - Allowing vendors to upload/download files from/to S3 or EFS              AWS SFTP    AWS AS2


## AWS DataSync vs AWS Transfer Family
*(Slide 1104)*

AWS DataSync                              AWS Transfer Family
- Used for bulk data transfer, data synchronization.
- Used for interactive file transfers and
- Common for on-premises to AWS or AWS to AWS               designed for end users, partners, and legacy transfers for large-scale migrations.                     systems.
- Supports NFS and SMB protocols
- Replaces traditional FTP/SFTP servers.
- Support Amazon S3, EFS, and FSx as storage.
- Supports file transfer protocols SFTP, FTPS, FTP, or AS2.
- Typically, one-time or scheduled, not for user-driven applications and uploads.                               • Provides managed endpoints backed by S3 or EFS.
- Not used by end users, hence user access is not a concern                                                 • User authentication and access control are key features.


## AWS Storage Gateway
*(Slide 1105)*

*Hybrid storage*


## Access data in AWS from on-premises
*(Slide 1106)*

```text
                                       Online
                                                     AWS
                                                   DataSync



                                                     How to
                                                   access the
                                                     data?
               Customer
               Datacenter




                                     Offline
                                                  AWS       AWS        AWS Data
                                                Snowcone   Snowball Transfer Terminal
```


## Access data in AWS from on-premises
*(Slide 1107)*

*Customer                       AWS Storage*

*Datacenter                      Gateway*


## Access AWS storage from on-premises
*(Slide 1108)*

```text
                                                                                   AWS Cloud
              On-premises
                                                                                                Storage services


    Application
                                NFS                           Storage Gateway                        Amazon S3 (+
    server
                                        Standard protocols       appliance                           Glacier)


                                SMB
    User
    Workstations                                                                                     Amazon FSx

                                                                                      Storage
                                                                                      Gateway
                               iSCSI                         Local cache for low
     Backup                                                                           Service           EBS
     Server                                                    latency access                         Snapshots
```


## AWS Storage Gateway
*(Slide 1109)*

- AWS Storage Gateway connects on-premises applications to AWS cloud storage services
- AWS Storage gateway appliance can be deployed on local Virtual Machines or on EC2 instance in AWS S3 File Gateway
- Supports storage protocols like NFS, SMB, iSCSI
- There are following different storage gateway types:
  - Amazon S3 File Gateway - Access data in S3 (e.g. logs, images, videos etc.) FSx File Gateway
  - Amazon FSx File Gateway – Access data in FSx for Windows Server (File access)
  - Volume Gateway – Provides cloud-backed storage volumes as iSCSI devices
    - Cached Volumes – Caches frequently accessed data locally and store rest of the data in S3 Volume Gateway
    - Stored Volumes – Low latency access to full data and asynchronous backup to S3
  - Tape Gateway - Replace physical tapes with virtual tapes in AWS S3/Glacier (Tape backup)
Tape Gateway


## AWS Backup
*(Slide 1110)*

- Centralizes and automates backups for data across AWS services and on-premises.
- Uses Backup plans to configure Frequency of the backup, backup time window, AWS resources to backup, storage tier for backup etc.
- Supports lifecycle management policies to move backups into low cost (cost) storage
- Supports Cross-region backups where backups are stored in another region to recover from region level failure
- Supports Cross-account backups                                                                   AWS Backup
- Backups are incremental (only first backup is a fully copy) for the supported resources            Service
- Integrates with most of the AWS services and external applications such as:
and more..
On EC2 EC2             EBS              S3   DynamoDB   RDS         EFS         FSx     Storage Gateway


## Let’s talk architecture
*(Slide 1111)*


## Scenario and customer requirements
*(Slide 1112)*

1. A company has TBs of file data hosted on Windows file servers running on premises. This data is actively used every day by users and applications.
2. The company is in the process of migrating its Windows workloads to AWS.
3. Migration will happen gradually.
4. During this transition, users and applications must be able to access both on-premises and AWS- based file storage with minimum latency.
5. The company has private network connectivity to AWS.
The solution must:
- Provide low-latency file access during the migration phase
- Preserve existing file access patterns so users do not need to change how they work
- Minimize operational overhead, avoiding self-managed synchronization or complex tooling


## Current architecture
*(Slide 1113)*

```text
         On-premises                                      AWS Cloud

              Application servers




                                              VPN or DX




               Windows File Server
```


## Solution Design
*(Slide 1114)*

1. Deploy Amazon FSx for Windows File Server in AWS to provide a highly available, durable, and managed file system for cloud workloads.
2. Deploy Amazon FSx File Gateway in the on-premises environment to expose the same file system locally with low-latency access.
3. Start moving the existing on-premises file data to the FSx File Gateway so it is stored centrally in FSx while remaining accessible on premises. Use AWS DataSync service for moving the data.
4. Configure AWS-based workloads (EC2) to directly access FSx for Windows File Server in AWS.


## Solution architecture
*(Slide 1115)*

```text
         On-premises                                                                         AWS Cloud

              Application servers




                                                                         VPN or DX

                                                                   accessing cloud storage
                                                    Amazon FSx
                                                                                               FSx for Windows
                                                    File Gateway
                                                                                                 File Server
                                              SMB
               Windows File Server                                    Online data transfer
                                                DataSync Agent
```


## Let’s talk architecture
*(Slide 1116)*


## AWS DataSync – Network connectivity options
*(Slide 1117)*

*Source: https://docs.aws.amazon.com/datasync/latest/userguide/networking-datasync.html*


## AWS DataSync – Network connectivity options
*(Slide 1118)*

```text
        Over the internet


                   On-premises                                                    AWS Region


                                                           Internet
                                                                                                  Public endpoint
                                                         (TLS security)   Control plane
                                                                             traffic


                                                                          Data plane traffic
                  Shared FS,                  DataSync
                                                                                               AWS DataSync         Amazon S3
                 object storage,              agent(s)
                   or Hadoop
                     cluster
```


## AWS DataSync – Network connectivity options
*(Slide 1119)*

```text
        Over the VPN conenction


                   On-premises                                              AWS Region



                                                            Internet                     DataSync
                                                         (IPSec security)                  ENIs
                                                                            Data plane
                                                                              traffic


                                                                                                                   Amazon S3
                                                         VPN connection
                  Shared FS,                  DataSync
                                                                              Control plane
                 object storage,              agent(s)                           traffic
                   or Hadoop                                                                  VPC endpoint
                     cluster                                                                                 AWS DataSync
```


## AWS DataSync – Network connectivity options
*(Slide 1120)*

```text
        Over the Direct Connect – Public VIF


                   On-premises                                                        AWS Region



                                                                  Public VIF
                                                                               Control plane
                                                                                  traffic


                                                                               Data plane traffic

                  Shared FS,                  DataSync   Direct Connect                     AWS DataSync   Amazon S3
                 object storage,              agent(s)
                   or Hadoop
                     cluster
```


## AWS DataSync – Network connectivity options
*(Slide 1121)*

```text
        Over the Direct Connect – Private VIF or DX gateway


                   On-premises                                                    AWS Region



                                                                                               DataSync
                                                                                                 ENIs
                                                                    Private VIF   Data plane
                                                                                    traffic


                                                                                                                         Amazon S3
                  Shared FS,                  DataSync   Direct Connect
                                                                                    Control plane
                 object storage,              agent(s)                                 traffic
                   or Hadoop                                                                        VPC endpoint
                     cluster                                                                                       AWS DataSync
```


## AWS DataSync – Network connectivity options
*(Slide 1122)*

*Source: https://docs.aws.amazon.com/datasync/latest/userguide/networking-datasync.html*


# Migration and Disaster Recovery

*(Source: Slide 1123)*


## Migration from on-premises to AWS
*(Slide 1124)*


## Migration 7R strategy
*(Slide 1125)*

```text
                                                                        7R’s
      Workload

                                                                        Retain          Applications not ready to migrate

                             Yes                   Preserve
        Need                             How                  Where    Relocate    VMWare on-premises to VMWare on AWS


                                                                        Rehost      Lift & Shift. Move to AWS without making
      No                                      Replace                                     any changes to the application

                                                                      Replatform     Move to Platform as a Service (PaaS)


                                                                      Repurchase     Drop and Shop. Replace with different
                                                              How                        version or product. Ex: SaaS

                                                                       Refactor    Re-architect e.g. Monolith to microservices,
                                                                                          adopt cloud native services

                                                                        Retire         Drop. There is no business value in
                                                                                       retaining the application or moving
```


## Migration 7R strategy
*(Slide 1126)*

```text
                                                                        7R’s
      Workload
                                                                                   Host applications on-premises or on AWS
                                                                        Retain         Applications not ready to migrate
                                                                                                    Outpost
                             Yes                   Preserve
                                                                                   On-premises Kubernetes to EKS, VMWare
        Need                             How                  Where    Relocate    VMWare on-premises to VMWare on AWS
                                                                                              to VMWare AWS

                                                                        Rehost      Lift & Shift. Move
                                                                                      On-premises      to AWS
                                                                                                    Servers  to without
                                                                                                                Amazonmaking
                                                                                                                         EC2
      No                                      Replace                                     any changes   to the application
                                                                                                     instance

                                                                                   On-premises databases to Amazon RDS,
                                                                      Replatform     Move to Platform as a Service (PaaS)
                                                                                   Windows to Linux, Containers to ECS/EKS

                                                                      Repurchase      Drop and Shop. Replace with different
                                                              How                  GitHub or Confluence on-premises to SaaS
                                                                                          version or product. Ex: SaaS

                                                                       Refactor    Re-architect
                                                                                   Oracle DB toe.g. Monolith
                                                                                                Aurora       to microservices,
                                                                                                       Postgres  DB, Monolith
                                                                                           adopt cloud
                                                                                            app to     native services
                                                                                                   containers (ECS)

                                                                        Retire        Drop. There is no business value in
                                                                                      retaining the application or moving
```


## AWS Migration Services
*(Slide 1127)*


## Portfolio
*(Slide 1128)*

```text
                 Assessment
                                                                          AWS Application
                                              Migration Evaluator                                      AWS Migration Hub
                                                                      Discovery Service (ADS)

                               On-premises

                      Servers                                                                                         EC2
                                                          Lift and Shift


                                                                                      AWS Application
                                                                                  Migration Service (MGN)
                    Databases                                                                                     RDS / Aurora
                                                          Replatform Databases


                                                                                 AWS Database Migration
                                                                                  Service (AWS DMS)
                     Web Apps                                                                                  ECS/EKS/Fargate
                                                          Replatform Containers


                                                                                     App2Containers
```


## AWS Migration Services – Updates (Nov 25)
*(Slide 1129)*

AWS Migration Hub                                  •   Agentic AI-driven modernization
- Multi-workload support (mainframe, Windows/.NET, VMware, custom)
- Automated code refactoring
- Discovery & assessment with dependency analysis AWS Application                AWS Transform
- Integrated testing and validation Discovery Service (ADS)                            •   Generation of modern deployment artifacts
- Pre-built and custom transformation agents
App2Containers


## AWS Migration Services
*(Slide 1130)*

AWS Application                      AWS Migration Hub Discovery Service (ADS)
- Capture system and
- Consolidate discovery data           AWS Application applications inventory and              across regions                   Migration Service (MGN) dependencies                        •   Visualize discovery data in
- Agent based and Agentless               QuickSight tools                               •   Plan for right sized EC2
- Connect and collect details             instances from on-premises IT systems,        •   Track the entire migration CMDB etc.                               centrally AWS Database Migration Service (AWS DMS) Agentless Collector
Discovery Agent


## AWS Application Migration Service (MGN)
*(Slide 1131)*

- Lift-and-shift (rehost) migration service for servers to AWS                                                                                   AWS Application Migration Service (MGN)
- Replication Agent must be installed on each source VM/physical server (on-premises or other clouds)
- Performs continuous block-level replication of server                                      (Staging area) disks
- Replicated data is compressed and encrypted in transit Volume
- Uses an AWS-managed staging area with temporary EC2 + EBS volumes Replication        Replication
- Supports Test migrations and Cutover migration             Server     agent              Server launches.                                                                                            Volume
- Maintains near-zero data loss (low RPO) and No downtime during replication phase


## MGN for On-premises to AWS
*(Slide 1132)*


## MGN for AWS to AWS across regions
*(Slide 1133)*


## How to migrate existing database to AWS?
*(Slide 1134)*

```text
                         On-premise

                                              Like to Like

                                                             RDS (Postgres)
                                                                                    Homogeneous
                                              Like to Like                            migration


                                                              RDS (Oracle)


                                                 Convert                         Heterogenous
                                                                                   migration

                                                             Aurora (Postgres)
```


## Amazon Database Migration Service - DMS
*(Slide 1135)*


## Schema conversion tool
*(Slide 1136)*

```text
          Create
                                              Migrate
          Target
                                              Schema
         Database




                                     Application


                                                        SCT
                                               Schema         Schema




                                                               Target
                                        Source
                                                              Database
                                       Database
```


## Database migration steps
*(Slide 1137)*

```text
          Create                                                  Set up                             Setup Change          Switch
                                              Migrate                             Initiate full
          Target                                                replication                          Data Capture        Application
                                              Schema                             load of data            (CDC)
         Database                                                process                                                 to new DB




                                     Application



                                                                              Replication Instance

                                                        Full load

                                                        CDC
                                                                              Replication task
                                                                                                                     Target
                                        Source
                                                                                                                    Database
                                       Database
```


## Database migration steps
*(Slide 1138)*

```text
          Create                                                  Set up                             Setup Change          Switch
                                              Migrate                             Initiate full
          Target                                                replication                          Data Capture        Application
                                              Schema                             load of data            (CDC)
         Database                                                process                                                 to new DB




                                     Application

                                                                                    MGN

                                                                              Replication Instance

                                                        Full load

                                                        CDC
                                                                              Replication task
                                                                                                                     Target
                                        Source
                                                                                                                    Database
                                       Database
```


## DMS replication instance
*(Slide 1139)*

```text
                                                  Replication Instance

                                      Full load

                                        CDC
                                                   Replication task       Target
              Source                                                     Database
             Database
```


## DMS Replication instance
*(Slide 1140)*

- The DMS replication instance is an EC2-based server that reads from the source database and writes to the target database.
- AWS DMS supports multiple instance families for the replication instance:
  - General purpose (T3) – Balanced CPU, memory, network; good for small workloads or testing
  - Compute optimized (C5, C6i, etc.) – Higher CPU performance for compute-heavy tasks
  - Memory optimized (R5, R6i, etc.) – More RAM per vCPU for memory-intensive migrations
- How to decide instance family?
    - Memory-optimized instances (R5/R6i etc.) are better for migrations with high throughput (read/writes), high CDC and transactions, or large amounts of in-memory data.
    - Compute optimized instances (C5/C6i etc.) are suited for compute-intensive tasks, such as handling transformations or processing many parallel tasks.


## DMS replication instances - Multi-AZ
*(Slide 1141)*

```text
                                                    Region

                                                       Availability Zone 1            Availability Zone 2



                                                                   Replication Instance

                                      Full load

                                        CDC
                                                                  Replication task                           Target
              Source                                                                                        Database
             Database                                Replication Instance            Replication Instance
                                                          (Primary)                       (Standby)



                                                  DMS Multi-az deployment – Synchronous replication
```


## MGN vs DMS
*(Slide 1142)*

VS AWS Application                           AWS Database Migration Service Migration Service (MGN)                               (AWS DMS)
- Migrates on-premises databases to
- Replicate application and Amazon RDS, Aurora, Amazon Redshift or databases to Amazon EC2 DynamoDB databases
- It’s a server based like to like
- Supports both homogeneous database migration migration (like to like) and heterogeneous migration (e.g. SQL server to Aurora Postgres) using SCT (Schema Conversion Tool)
Rehost (Lift-and-Shift)                                  Replatform


## Disaster Recovery
*(Slide 1143)*


## What is Disaster?
*(Slide 1144)*

- Natural disasters, such as earthquakes or floods
- Technical failures, such as power failure or network connectivity
- Human actions, such as inadvertent misconfiguration or unauthorized/outside party access or modification
Delete
Database


## Disaster Recovery (DR)
*(Slide 1145)*

The process of preparing for and recovering from a disaster
DR Objectives:
- Recovery point objective (RPO): The maximum acceptable amount of time since the last data recovery point.
- Recovery time objective (RTO): The maximum acceptable delay between the interruption of service and restoration of service.
RPO=1 week RTO=2 days Backup 1                   Backup 2
New data
(Next planned backup)
01-Sep                    08-Sep                 13-Sep            15-Sep             Time


## DR Strategies
*(Slide 1146)*

```text
                                 Active/Active                                   Active/Passive



                                                      After disaster                                 After disaster


                   Active Site                 Active Site             Primary Site         Passive Site




                                 Replication                                                        OR

                                                                       Primary             Backup          Standby


                                                                                                           Source: AWS blog/
```


## DR Strategies                                          Active/Passive
*(Slide 1147)*

Backup and restore                                         Pilot Light                                  Warm Standby
Primary Site                          AWS Region      Primary Site               AWS Region        Primary Site               AWS Region
copy backup                                                      Replicate backup                                                                            Replicate
Before the event                                          Before the event                           Before the event
- Take regular backups
- Replicate data
- Replicate data
- Keep Infrastructure elements
- Keep infrastructure running After the event                                              ready but not On                           at lower capacity
- Provision infrastructure resources                                             After the event                            After the event
- Restore from backups
- Bring-up compute
- Scale the capacity


## DR Strategies vs RPO/RTO
*(Slide 1148)*

*Source: AWS Blog*


## Let’s talk architecture
*(Slide 1149)*


## DR Strategies
*(Slide 1150)*

*On-premises to AWS                      AWS to AWS*

*Region   Region 1            Region 2*


## Active/Active                                          traffic                     traffic
*(Slide 1151)*

DNS query
- Possible if both sites are in AWS (in   Region 1                                     Region 2 different regions)
- Re-routing the requests to healthy ELB                                          ELB region Route 53
- Data is replicated in real-time
- RPO is near zero
- RTO in seconds
- Cost is very high due to multiple sites
Automatic replication (Aurora Global Database) Amazon Aurora                                 Amazon Aurora RTO            RPO               COST


## Backup & Restore
*(Slide 1152)*

On-premises to AWS
- Regularly backup the Application Servers data, File                                     AWS Region
servers and Databases
- Move backups to AWS S3 (using Storage gateway,                                                                      ELB Snowball etc.)                                          AWS
- In case of disaster, launch                           DataSync EC2/RDS and restore data /                                            S3 and    Restore databases                                                             Glacier
Backups       AWS Storage Gateway
EBS snapshots AWS Snowball                            Amazon RDS RTO            RPO               COST


## Backup & Restore                                                                                           AWS
*(Slide 1153)*

```text
                                                                           AWS to AWS                         CloudFormation

                                              AWS Region                                   AWS Region




                                                                                                                     ELB
                                                  ELB          AMI                          AMI

                                                                              Copy                      Restore



                                                           EBS Snapshots                EBS Snapshots




                                                           RDS Snapshots                RDS Snapshots             Amazon RDS

  RTO              RPO               COST
```


## Amazon RDS Multi-AZ: HA or DR?
*(Slide 1154)*

```text
                                                               Region
      Answer: Highly Available (HA)
                                                                 Availability Zone 1              Availability Zone 2




   If by mistake someone drops important table, then it will                           Multi-AZ
   be dropped from the secondary DB as well. In this case
   application will be available but won’t be functional
   causing a disaster.
                                                                Amazon RDS                           Amazon RDS
```


# AWS Multi-Account Management

*(Source: Slide 1155)*

*AWS Organizations, AWS Control Tower, IAM Identity Center and more..*


## AWS Organizations
*(Slide 1156)*


## AWS accounts in an enterprise
*(Slide 1157)*

```text
                                        Project A                                  Project B                   Company
                    Development               Staging   Production   Development   Staging     Production
                                                                                                            Finance   HR
```


## AWS accounts in an enterprise
*(Slide 1158)*

```text
                                        Project A                                  Project B                   Company
                    Development               Staging   Production   Development   Staging     Production
                                                                                                            Finance   HR
```


## AWS Organizations
*(Slide 1159)*

```text
                                        Project A                              Project B                   Company
                      Development Staging           Production   Development   Staging     Production
                                                                                                        Finance   HR
```


## AWS Organizations
*(Slide 1160)*

```text
                                                                    AWS organization

                                 Consolidated Billing                                                SCP




                                 Project A Organization Unit           Project B Organization Unit
                                 (OU)                                  (OU)
                        Development Staging          Production   Development Staging      Production
                                                                                                           Finance   HR
```


## AWS Organizations
*(Slide 1161)*

```text
                                                                    AWS organization




                                                                                                        IAM Identity Center



                                 Project A Organization Unit           Project B Organization Unit
                                 (OU)                                  (OU)
                        Development Staging          Production   Development Staging      Production
                                                                                                          Finance       HR
```


## AWS Organizations Overview
*(Slide 1162)*

AWS Organization
- AWS Organizations is a global service used to centrally manage multiple AWS accounts.​
- It consists of one management account and multiple member accounts.​                                                         Management Account
- Each member account can belong to only one organization.​ OU
- Member accounts can be grouped into Organizational Units (OUs) for easier management.​ OU
- OUs support up to five levels of hierarchical nesting to meet security, compliance, and budgeting needs.
- Each organization has a unique Organization ID Member    Member
- AWS Organizations supports either “Consolidated billing only” or                     Account   Account “All features”


## AWS Organizations - Enabling all features
*(Slide 1163)*

- When all features are enabled, AWS Organizations can:
  - Apply Service Control Policies (SCPs)
  - Enforce Tag Policies
  - Manage Backup policies
  - Use AI services opt-out policies
  - Control access and governance across accounts
  - Enforce guardrails at:
    - Root
    - OU                                                     AWS Organization
    - Account level
- This mode turns Organizations into a governance and control plane, not just Consolidated billing.
- Once enabled, can not be rolled back


## AWS Organizations - Important features
*(Slide 1164)*

- Service Control Policies (SCP)
- Consolidated Billing
- Tag Policies
- Using aws:PrincipalOrgID in IAM resource-based policies   AWS Organization


## Service Control Policies (SCP)
*(Slide 1165)*

- Manage IAM permissions across AWS Accounts in AWS Organization.                    Management Account
- SCP governs the maximum available permissions for the IAM users and roles Root
- SCPs do not grant additional permissions to the IAM users and IAM roles.
- Applied at the Root level or OU level or Account level.
- SCPs don't affect users or roles in the management account. They affect only the                OU             OU member accounts in your organization.
- By default, FullAWSAccess is attached at root to allow all actions unless denied
- Common use cases: OU             OU
  - Restrict access to certain services which are banned in your organization (for example: AI services)
  - Restrict access to certain AWS regions where you do not have any workloads                                                                                Account       Account
  - Restrict access to certain instance types and size in the lower environments


## SCP policy evaluation and effect
*(Slide 1166)*

FullAWSAccess {
- SCP effective permissions are the intersection of all SCPs                             "Effect" : "Allow", attached at:                                                                           "Action" : "*", "Resource" : "*“ } Root { "Effect" : "Allow", "Action" : “ec2:*", OU       "Resource" : "*“ }
- Explicit Deny in any SCP always overrides everything else {
- If an SCP contains Allow statements, any action not explicitly                         "Effect" : "Allow", allowed is implicitly denied.                                                          "Action" : “S3:*", OU       "Resource" : "*“
- If no SCP is attached at a level, permissions are inherited                        } from the parent
- SCPs affect all IAM principals in the account: IAM users, IAM { Roles and Root user "Effect" : "Allow", "Action" : "*", "Resource" : "*“ What will be resulting permissions at Account level?        Account        }


## Sample SCP policies                                                                 Apply at Root level or OU level
*(Slide 1167)*

*Restrict all users to launch only t2.micro instance type*

*https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_examples.html*


## Sample SCP policies
*(Slide 1168)*

```text
                                                                                            Apply at Root level or OU level




       Restricts all users from uploading unencrypted objects to
       S3 buckets.




                                                        https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_examples.html
```


## Sample SCP policies
*(Slide 1169)*

```text
                                                                                           Apply at Root level or OU level




         Restricts all users to use only Mumbai region to
         launch their AWS resources




                                                            https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_examples.html
```


## AWS Organizations – Consolidated Billing
*(Slide 1170)*

```text
                                                                     Monthly Consolidated Bill


                                              AWS Account A     Paying Account       $38.20


                                                                AWS Account A        $8.68
             Paying Account
                                              AWS Account B
                                                                AWS Account B        $138.70

                                              AWS Account C     AWS Account C        $18.10


                                                                AWS Account D        $23.40
                                              AWS Account D
                                                                Total Charged to
                                                                Paying Account       $227.08
                                              Linked Accounts
```


## AWS Organizations – Consolidated Billing
*(Slide 1171)*

- Management account to pay bill for entire AWS organization (including all member accounts)
- Benefits:
  - One Bill - Get one bill for multiple accounts.
  - Combined Usage - Combine the usage across all accounts in the organization to share the volume pricing discounts, Reserved Instance discounts, and Savings Plans. This can result in a lower             A        B            C       D charge for your company.
  - No extra Fee - Consolidated billing is offered at no additional cost. Account A consumed       Remaining $5/hr Account A purchased only $5/hr EC2         commitment is EC2 savings plan capacity throughput   applied to other AWS $10/hr commitment the month         accounts in the AWS Organization


## Exercise : Create new member account and apply SCP
*(Slide 1172)*

```text
           1     Login into AWS Management account and go to AWS Organizations                              Management account steps

           2     Under Root OU -> Create a new OU (say Development)                                         Member account steps


           3     Under Development OU -> Create a new AWS account (provide email id)

           4     After account created successfully, login to new account in new browser private window with email id. You
                 would have to choose forgot password option for first time login.

           5     In the management account, go to SCP and create a new SCP policy which restricts launching only EC2
                 t2.micro instances. Refer to the policy document in the next slide.

           6     Attach this policy to the Development OU that you created earlier. It should be automatically applied to the new
                 AWS account that you created.

           7     In the new AWS account, try to launch t2.large EC2 instance. Access should be denied. But now try launching
                 t2.micro EC2 instance, it should be launched successfully.

           8     If you do not need a new account that you created, then Close the account from management account screen.
```


## Sample SCP policy:
*(Slide 1173)*

```text
           {
               "Version": "2012-10-17",
               "Statement": {
                   "Effect": "Deny",
                   "Action": "ec2:RunInstances",
                   "Resource": "arn:aws:ec2:*:*:instance/*",
                   "Condition": {
                       "StringNotEquals": {
                           "ec2:InstanceType": "t2.micro"
                       }
                   }
               }
           }
```


## AWS Organizations – Tag policies
*(Slide 1174)*

- Used to standardize resource tagging across all AWS accounts in an organization
- Define rules for tag keys and values, including:
  - Required capitalization and case (for example, CostCenter vs costcenter)
  - Allowed values for specific tag keys
- Enforcement modes:
  - Enforced → Blocks noncompliant tagging on specified resource types
  - Monitoring only → Allows tagging but flags noncompliance
- Compliance can be viewed or fixed using:                                                Key = Value
  - AWS Resource Groups
  - Tag Editor
  - Tagging API
- Tag policies require AWS Organizations all features enabled
- Tag policies do not grant or deny service permissions (they only govern tagging)


## IAM Resource Policies with aws:PrincipalOrgID
*(Slide 1175)*

{
- AWS Organization has unique Organization ID "Version": "2012-10-17", "Statement": [
- This organization ID can be used in IAM resource-based              { "Sid": "AllowGetObject", policies                                                            "Effect": "Allow", "Principal": {
- It ensures that only principals belonging to a specific AWS              "AWS": [ "*" ] }, Organization can access resources like S3 buckets, VPC              "Action": "s3:GetObject", "Resource": "arn:aws:s3:::finops-data/*", endpoints, or KMS keys etc.                                         "Condition": { "StringEquals": { "aws:PrincipalOrgID": ["o-ea3x9tl9cz"] } } } ] }


## AWS Control Tower
*(Slide 1176)*


## Multi-account guardrails and best practices
*(Slide 1177)*

```text
                                              AWS CloudTrail Security Hub          Transit Gateway N/W Firewalls




                      Development Staging             Production     Development      Staging    Production        Finance   HR


                                       Project A                                     Project B                           Company
```


## Multi-account guardrails and best practices
*(Slide 1178)*

- How to deploy this kind of Multi-account setup from scratch which provides systematic, structured way to create, manage and deploy AWS accounts, SCP policies, IAM users, create cloudtrail trails, config rules etc.?
- How to ensure that every new account provisioned is setup with these security best practices and guardrails?
Enter.. In less than an hour.. AWS Control Tower
AWS Landing Zone


## AWS Control Tower
*(Slide 1179)*

- Landing Zone: A well-architected, multi account-environment that's based on security and compliance best practices.
  - Mandatory guardrails: Disallow root access, disallow changes to encryption, enforce MFA, disallow public access to S3 buckets etc.
  - Elective guardrails: Disallow SSH/RDP from internet, Enforce tagging, Disallow RDS snapshot public sharing etc.
- Works with AWS Organizations, AWS IAM Identity Center, AWS CloudFormation & Service Catalog.                                              AWS Control Tower
- Account Factory: A template for creating new accounts with pre-configured resources and controls e.g. Security groups, VPC, CloudTrail trail etc.
- Dashboard: Centralized view of entire Landing zone, compliance policies, non-compliant resources organized by accounts and Ous.


## AWS Landing zone provisioned by AWS Control Tower
*(Slide 1180)*

*https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/building-landing-zones.html*


## IAM Identity Center
*(Slide 1181)*


## AWS IAM Identity Center
*(Slide 1182)*

```text
                                                                                                   AWS Organization




                                              Login                                                    AWS Accounts
                                                                                  Single Sign-on


                                                                IAM Identity
                                                                  Center

                                                          User
                                                      Authentication

      IDPs



                                                                         Identity Center
                                                                            Directory
```


## IAM Identity Center – Key Features
*(Slide 1183)*

User logins
- Centralized (SSO) login for all accounts in AWS Organization
- Centralized (SSO) login for all business applications
Identity Provider (IdP)
- Built-in Identity Center Directory
- Integration with AWS Managed Microsoft Active Directory
- Integration with on-premises Microsoft Active Directory                                  AWS IAM Identity Center
- External IdPs via SAML2.0
Permission Sets
- Permission sets define what users can do in AWS accounts.
- Internally translated into IAM roles in each target account.
- Assigned to Users or Groups in Identity center. In 2022, AWS Single Sign-On was renamed to AWS IAM Identity Center.


## IAM Identity Center integration with Active Directory
*(Slide 1184)*

  - Use AWS Managed Microsoft Active Directory (AD)
IAM Identity Center         AWS Managed Microsoft AD
- Use Self-managed Microsoft Active Directory (AD)
Two-way trust
AWS Managed                     DX or VPN Microsoft AD IAM Identity Center                                    proxy
AD Connector


## How Permission sets work?
*(Slide 1185)*

AWS Organization IAM Identity Center                                        Login
Permission Set                  Permission Set           User
- AdministraorAccess
- BillingReadOnly Account
assign             assign         assign           assign Workload OUs and Accounts
Account A User Group (admins)
Account B                               Assume role
User (Bob)


## Exercise: Login to AWS account using IAM Identity Center
*(Slide 1186)*

AWS Organization IAM Identity Center       Login
Permission Set
AWS Management                     • ReadOnly Identity Center Account Directory assign            assign Development Account
Assume role
User


## AWS Resource Access Manager
*(Slide 1187)*


## AWS Resource Access Manager (RAM)
*(Slide 1188)*

- Share AWS resources with other AWS Accounts     AWS Organization
- In AWS organization, resource can be shared       Account A              Account B             Account C (Resource Owner)     (Resource consumer)   (Resource consumer) with a single account or OU or all accounts
- Resource can also be shared with AWS account       Private subnet
outside of AWS organization
- Supports AWS services and resources like VPC subnets, Transit gateway, Route53, EC2 dedicated host, CloudHSM and more.. FSx for OpenZFS
AWS CloudHSM


## IAM Advanced
*(Slide 1189)*


## IAM Roles and AWS STS
*(Slide 1190)*

- IAM roles do not have long-term credentials.
- When a role is assumed, AWS STS (Security Token Service) issues temporary security credentials for that role.
- These credentials include:
  - Access key ID
  - Secret access key
  - Session token
- The credentials are time-limited and automatically expire (typically 15 min – 12 hours).
- AWS STS provides APIs such as AssumeRole, AssumeRoleWithSAML, and AssumeRoleWithWebIdentity which provides temporary credentials.
- STS APIs to get temporary credentials:
  - AssumeRole - For AWS-to-AWS (EC2/Lambda accessing S3 etc.)
  - AssumeRoleWithSAML - For Enterprise SAML 2.0 (Login with AD FS, Okta, Azure AD etc.)
  - AssumeRoleWithWebIdentity - For Web/mobile identity (Login with Google, Facebook, Cognito etc.)


## IAM Role types
*(Slide 1191)*

- Service IAM Role
- AWS Service-Linked IAM Role
- Cross-account IAM Role
- Federated IAM Role


## IAM Role types
*(Slide 1192)*

- AWS Service Role
  - A role that an AWS service assumes to perform actions on your behalf. Role
  - Used when an AWS service (like EC2, Lambda, or ECS) Lambda needs permissions to access other AWS resources.
  - You define what the service can do by attaching policies to the role.
  - Examples:                                                                    sts:AssumeRole STS
    - EC2 instance reading/writing to S3
    - Lambda writing to S3, CloudWatch Logs
    - Lambda reading/writing to DynamoDB S3


## IAM Role types
*(Slide 1193)*

Role
- Service-Linked Role
  - A special type of service role that is created and                                         Elastic Beanstalk managed by AWS for specific services.
  - Automatically includes predefined permissions needed by that service.
  - You can view or delete it but cannot modify its            Configuring                            Create load balancers                      CloudWatch Alarm policy. Adjust Scaling
  - Example services that create these:                                         capacity
    - AWS Config
    - Amazon RDS
    - Elastic Beanstalk
Application Load     Auto Scaling           CloudWatch Balancer             Group


## IAM Role types
*(Slide 1194)*

Account A
- Cross-account Role
  - Allows entities in another AWS account to assume the role and access resources in your account.
  - Requires a trust policy specifying the external account or principal.
  - Example:                                                                         sts:AssumeRole
    - Account A’s user Ben assumes a role in Account B to    STS read from an S3 bucket.
Account B (Customer)


## IAM Role types
*(Slide 1195)*

- Federated Role
  - Used when external identities (e.g., Active Directory, Corporate Google Workspace, Okta, etc.) need temporary access Users to AWS.
  - Users authenticate with an identity provider (IdP) using        sts:AssumeRoleWithSAML SAML 2.0 or OIDC, then assume the role through STS.
      - AssumeRoleWithSAML                                            sts:AssumeRoleWithWebIdentity
      - AssumeRoleWithWebIdentity                           STS
    - Enables SSO (Single Sign-On) without creating IAM users.


## IAM Federated Role vs IAM Identity Center
*(Slide 1196)*

IAM Federated vs IAM Identity Center
- For using IAM Federated role, you use AWS STS that issues temporary credentials as per role for a given AWS account.
- IAM Identity Center is a managed SSO service that uses STS internally to provide centralized, organization-wide access across multiple AWS accounts.   IAM Identity Center
- With STS, you manage IAM roles and federation manually; with IAM Identity Center, AWS manages role creation and assignment using permission sets.


## Exercise: IAM cross-account role
*(Slide 1197)*

```text
                   Account A                        Account B


                            IAM Policy                          S3ReadOnly
                        (sts:AssumeRole)
                                              STS




                                                          Cross-
             IAM User                                    account             S3 bucket
                                                        IAM Role
               (Ben)
```


## Exercise: Cross-account access
*(Slide 1198)*

Pre-requisites:
1. There should be two AWS Accounts. You may work with your friend for Account B if don’t have two accounts.
2. In Account B, there should be existing S3 buckets. We will just verify the list bucket permissions.
1      In Account B, login as an admin user and create cross-account IAM Role (say CrossAccountRoleForAccountA) and associate AmazonS3ReadOnlyAccess policy. Provide the Account A ID while creating this cross-account role. 2      In Account A, login as an admin user and create an IAM Policy to grant user permission to AssumeRole in Account B and attach this policy to any of the IAM user in Account A (Ben) {                                                                                                IAM Policy "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": "sts:AssumeRole", "Resource": "arn:aws:iam::<AccountB>:role/CrossAccountRoleForAccountA" } ] }


## Exercise: Verify cross-account access through AWS
*(Slide 1199)*

```text
         console

         1     Login in Account A as a normal IAM user (Ben) -> Go to Account ID (right corner) -> Add Session -> Switch
               Role
         2     Provide the details of Account B ID and cross-account Role name (CrossAccountRoleForAccountA) and
               select the color. If successful, you will see Ben user logged into Account B. Verify the user details – It should
               show federated user.
         3    Go to S3 service and check if you are able to see the existing buckets in Account B.
```


## Exercise: Verify cross-account access through AWS CLI
*(Slide 1200)*

```text
         1     From your command line (for user Ben in Account A), run the following command to AssumeRole in Account B and get
               the temporary IAM credentials.

              aws sts assume-role --role-arn arn:aws:iam::<AccountB>:role/CrossAccountRoleForAccountA                              -
              -role-session-name AccountBSession

             The AWS STS will return the Temporary security credentials including Access Key ID, Secret Access Key and Session Token


         2     Set the environment variables for temporary credentials

              Linux or mac:
               export AWS_ACCESS_KEY_ID=<access key id>
               export AWS_SECRET_ACCESS_KEY=<secret access key>
               export AWS_SESSION_TOKEN=<session token>

              Windows CMD: $env: for windows powershell and set of windows cmd

               set AWS_ACCESS_KEY_ID=<access key id>
               set AWS_SECRET_ACCESS_KEY=<secret access key>
               set AWS_SESSION_TOKEN=<session token>
```


## Exercise: Verify cross-account access through AWS CLI
*(Slide 1201)*

*3    Try to run the S3 real-only commands. These will be executed against Account B resources*

*aws s3 ls           //should list the buckets in Account B*


## Amazon Cognito
*(Slide 1202)*

- An identity platform for web and mobile apps users.
- Instead of creating them an IAM user, you create a user in Cognito.
- Create a User pool when you want to authenticate and authorize users to your app or API.
- Create an Identity pool when you want to authorize authenticated or anonymous users to access your AWS resources.
Login
Amazon Cognito Applications                                                             Social Identity Provider
Login with Google, Facebook..


# AWS Billing and Cost Management

*(Source: Slide 1203)*


## How to estimate, view and analyze AWS cost?
*(Slide 1204)*

  - How to estimate cost of AWS                                                         https://calculator.aws/ AWS Pricing Calculator services for your architecture?
Billing and Cost
- View overall AWS monthly Bills            AWS Billing Dashboard                      Management console
  - Analyze the cost per service, region                                                   Billing and Cost Cost Explorer etc. and also see forecasted usage                                                   Management console
  - Granular level report of cost                   Data Export                          Download Legacy CUR Legacy: Cost and Usage Report 2.0                from S3 Bucket breakdowns. Use your BI tools to analyze cost. Visualize in QuickSight                                Query QuickSight Athena Redshift
Billing and Cost
  - How to track the free tier usage?      Free tier usage dashboard                      Management console


## AWS Pricing calculator
*(Slide 1205)*


## AWS Bills
*(Slide 1206)*


## AWS Cost explorer
*(Slide 1207)*


## Free tier usage dashboard
*(Slide 1208)*

- AWS Billing and Cost Management console -> Cost Analysis -> Free tier


## How to track and control cost?
*(Slide 1209)*

- Notify when total usage exceeds                AWS Billing Alarm                                            Alarm certain threshold e.g. > $100 for current month                                                                                 Amazon CloudWatch
- Set cost budgets for overall usage or                                        Billing and Cost AWS Services e.g. $1000 for EC2,                 AWS Budgets               Management console $500 for S3 and get notified when exceeded
- Want to track cost by environments,                                              Tag Editor Cost Allocation Tags projects, services, users or based on                                      aws:CreatedBy different dimensions?                                                      user:Environment = Dev user:Owner = abc@xyz.com
- Want to get recommendations for             Cost Optimization Hub              Billing and Cost purchasing Savings plan, RIs, right                                          Management console
sizing and configuration of AWS         Savings Plan         AWS Compute resource (e.g. EC2, EBS etc.) ?                                Optimizer Reserved Instances


## Exercise: AWS Billing Alarm
*(Slide 1210)*

DO THIS IN N.VIRGINIA (us-east-1) REGION
Set a Billing alert so that you get notified when your AWS usage bill exceeds some threshold (say > $5/month)
1. Billing and Cost Management -> Billing Preferences -> Alert preferences -> Edit -> Receive CloudWatch Billing Alerts -> Save
2. In Amazon SNS -> Create a new topic and subscribe with your email id
3. In Amazon CloudWatch -> Go to -> Alarms -> Billing -> Create alarm Metric Name: EstimatedCharges Currency: USD Statistic: Maximum Period: 6 hours Threshold Type: Static Whenever EstimatedCharges is: Greater, than -> 5
4. Alarm State trigger: In alarm -> Send a notification to the following SNS topic -> Select an existing SNS topic -> Select the topic you created earlier -> next -> Alarm name: BillingAlarm5USD -> next -> Create alarm


## AWS Budgets
*(Slide 1211)*

*https://aws.amazon.com/blogs/aws-cloud-financial-management/beginners-guide-to-aws-cost-management/*


## Exercise: AWS Budget
*(Slide 1212)*

```text
         Go to Billing and Cost Management -> Budgets
          a. Use a template -> Monthly cost budget
          b. Provide Budget name and budget value
               in USD (e.g. 5)
          c. Provide the email id to which you should
               receive an email when usage exceeds
               the budgeted value
          d. Create budget


                                                        Email sent by AWS
```


## AWS Cost Optimization Hub – Savings plan
*(Slide 1213)*

*https://aws.amazon.com/blogs/aws-cloud-financial-management/beginners-guide-to-aws-cost-management/*


## AWS Cost Optimization Hub - Reservation
*(Slide 1214)*

*https://aws.amazon.com/blogs/aws-cloud-financial-management/beginners-guide-to-aws-cost-management/*


## Before you take the exam
*(Slide 1215)*


## Before you take the exam..
*(Slide 1216)*

- How to approach exam questions - Tips
- Getting 30 mins extra time for the exam (as applicable)
- Scheduling the exam
- Getting 50% discount on your next AWS certification exam


## Know your exam
*(Slide 1217)*

- Exam: AWS Certified Solutions Architect Associate
- Exam code: SAA-C03
- Total 65 questions
- Time: 130 Minutes
- 50 questions are scored and 15 are unscored
- There are two types of questions in the exam:
  - Multiple choice: Has one correct response and three incorrect responses
  - Multiple response: Has two or more correct responses out of five or more response options
- Minimum passing score: 720/1000
https://aws.amazon.com/certification/certified-solutions-architect-associate/


## How to approach exam questions
*(Slide 1218)*

*Multiple choice   Multiple responses*


## How to approach exam questions
*(Slide 1219)*

```text
       ✓ Don’t spend more than a minute and half for a question.
       ✓ If in doubt, select the answer which came first to your mind and Mark that question for review
       ✓ Even if you do not have any idea about the question, just answer it and Mark the question for review
       ✓ Try to finish the first pass over all 65 questions in around ~100 mins
       ✓ In last 15-30 mins, go over all the Marked for review questions and double check your answers.




                                                     Mark for review
```


## Getting 30 mins extra time for the exam
*(Slide 1220)*

- For non-native English speakers AWS provides 30 mins extra time
- You can request it through AWS certification portal -> Exam accommodations -> ELS + 30 accommodation
- Once opted, it will be there for all your future AWS certification exams


## Let’s connect
*(Slide 1221)*

*Please subscribe to YouTube channel              Connect with me on LinkedIn*

*https://www.youtube.com/@AWSwithChetan       https://www.linkedin.com/in/chetan-agrawal-30107310/*


## Thank you and All the best!
*(Slide 1222)*


---

*End of notes — All rights reserved © www.awswithchetan.com*
