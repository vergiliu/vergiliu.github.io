---
layout: post
title: "AWS (Solutions Architect Associate) by CloudGuru"
date: 2017-06-01 22:22:22
tags: [video, reviews, 2017, 2019]
rating: na
---

## 2019
### EC2 (Elastic Compute Cloud)
- pricing: on demand, reserved (1yr or 3yrs), spot, dedicated 
- spot terminated will not be charged if terminated by Amazon
- first launch AMI cannot be encrypted (root can be encrypted later )
- on EBS backed, default action is to terminate and delete the volume
- termination protection by default is OFF

#### EC2 Security Groups
- all changes to SGs take effect immediately
- SG are stateful, all inbound rules are added automatically to outbound rules
- we cannot blacklist a particular port or IP address
- 1 or more SGs can be assigned to an instance
- all inbound traffic (everything) is BLOCKED by default
- all outbound traffic is ALLOWED
- we can have any number of EC2 instances withing a single SG
- we can have multiple SGs to a single EC2 instance
- we can specifically ALLOW rules, but not DENY rules
- NACL are stateless, port or IP address can be blocked in NACLs

#### EBS (Elastic Block Store)
- persistent block storage
- 5 different flavours: general purpose SSD (16000 iops) - gps, provisioned IOPS (64000 iops) SSD io1, throughput HDD (500 iopsa) st1, Cold HDD (250 iops), EBS Magnetic
  - max size is 16TB
- ebs volume always on same AZ as the ec2 instance
- copy AMI to a different region by using snapshots
- additional volumes will continue to persist on termination, root volume will be terminated
- snapshots exist on S3, and are INCREMENTAL
- AMIs can be created from both volumes and snapshots
- you can change volume types on the fly
- AMI based on Region / OS / Arch / Launch Permissions / Storage for root device: Instance Store (ephemeral) / EBS Backed Volumes
  - EBS - from an EBS snapshot
  - Instance Store - root device created from template stored in S3
    - can be added only before startup of the AMI
    - sometimes called ephermeral storage
    - if instance is stopped, all data is lost
- EBS backed instance will not lose data on stop
- by default, both root volumes will be deleted on termination - for EBS backed the default can be changed

##### Encrypted Root Device volumes and snapshots
- an ec2 instance can be started with an encrypted root volume (new)
- to encrypt a volume - take a snapshot of the volume, copy the volume and select ENCRYPT, next create an IMAGE out of the encrypted snapshot -> that can be launched as an encrypted AMI
  - this cannot be launched as an unencrypted volume
  - snapshots of this will be encrypted automatically
  - volumes restored from encrypted snapshots are encrypted

#### CloudWatch
- monitors performance
- 5 minute intervals by default (can be set to 1minute)
- monitor service to check resources from AWS
  - host level metrics, CPU, network, disk, status
  - as well as applications
- CloudTrail monitors API - it's more about auditing than performance

### Region > AZ
- 19 regions & 57 AZs
- AZ think of it as a DataCenter
- Region: a geographical area, made up of 2 or more AZ
- Edge location: endpoints for caching content, used by CloudFront (CDN), ~ 150 edge locations currently
- 4 levels of support, 3 paid, TAM only on Enterprise level support

### Identity Access Management (IAM)
- manage users and level of access to the console
- shared access, granular permissions, Identity Federation
- MFA, password rotation policy
- temporary access for users, if necessary
- PCI DSS compliance
- key terms: Users, Groups, Policies (made from polici documents, JSON), Roles
- custom sign in domain
- IAM settings are all global, does not apply to regions
- best practice is to not directly use root account/"god mode"
- roles - allow one  service to use another service e.g. EC2 instance can use S3
- new users have _NO_ permissions when created 
  - key and secret ID assigned for programmatic access
  - user/password for console access, different than keys mentioned above
  - only visible once, make sure not to loose them
- always setup MFA on your root account
- password policies and rotation can be configured
- policies can be AWS or customer-managed
- CloudWatch can manage/create billing alarms
- SAML can give you federated SSO to AWS

### Compute

####  S3 - Simple Storage Service
- **read S3 FAQ before exam**
- object based storage, unlimited in size
- 0 bytes to 5TB, stored in buckets
- example URL https://s3-eu-west-1.amazonaws.com/<bucketname>
- buckets must be unique globally (universal name), http200 when upload ok
  - an S3 object has: **key** (name), **value** (file data), **versionId**, **metadata**, and **Subresources**
    - ACL , torrent
  - Consistency types for S3 objects
    - read after write consistency for PUTs
    - eventual consistency for overwrite PUTs and DELETEs
- MFA delete protection can be set up
- built for 4 9s availability, guaranteed 3 9s; 11 9s durability
- charges: Storage, Requests, Storage Mgmt, Data transfer, Transfer Acceleration (CloudFront edge locations), Cross region replication
- storage classes (tiers)
  - S3 standard - can sustain loss of 2 facilities
  - S3 Infrequent Access (IA)
  - S3 One Zone IA (reduced redundancy storage)
  - S3 Intelligent Tiering
  - S3 Glacier - for data archiving
  - S3 Glacier Deep Archive - 12hrs retrieval time
- Control to buckets can be set using bucket policies 
- by default all new buckets are _private_
- S3 buckets can be configured to store access logs from S3 buckets access
- ACL are at object level
- encryption levels: -
  - in transit (HTTPS), 
  - at rest (server side)
    - S3 managed keys SSE S3
    - AWS KMS (Key Mgmt Service) SSE KMS
    - server side with client keys SSE C
- Version Control
  - stores all versions, including writes
  - once you enable it, can only be suspended
  - integrates with Lifecycle rules
  - integrates with MFA Delete
- S3 lifecycle rules
  - can be used in conjuction with versioning, and applied to current and previous versions
  - automate transitions to different tiers of storage
  - transition and expiration to XX from YY after number of days
- Cross Region Replication
  - can be used only when versioning is enabled, on both source and replication buckets
  - the replicated S3 can be in a different tier
  - only changes after enabling CRR are visible in the replicated bucket
    - files in existing bucket are not replicated automatically, all subsequent updates are recorded
  - won't replicate delete markers nor deletes of _individual_ versions
- Transfer Acceleration
  - upload directly to Edge Location which in turn uploads to S3

#### CloudFront
  - it is a global service - CDN
  - Edge Location - location where content will be cached - not an AZ nor Region
  - Origin: ec2, elb, s3, route53 - origin of files distributed towards cdn
  - Distribution (name of CDN): collection of edge locations, name given to the CDN; it can have 2 types
    - Web - use for websites
    - RTMP for streaming / Adobe something
  - edge location can be written to (see transfer acceleration)
  - objects are cached for TTL (time to leave), can be invalidated - you will be charged
  - access can be restricted using sign URLs or cookies
  - in a Distribution we can add `Invalidations` in case we want to no longer cache certain content/files/file types

#### Snowball
- PB-scale data transfer 50 80TB
- Snowball edge 100TB
- Import and Export large amounts of data to S3

#### Storage Gateway
- connects an on-premise software appliance with AWS storage infra
- virtual (VMWare or Hyper-V) or physical device as a Storage Gateway
- 3 types :
  - File Gateway (NFS, SMB) -> flat files, S3
  - Volume Gateway (iSCSI) -> EBS snapshots to S3
    - Stored Volumes -> store your primary data locally (on site)
    - Cached Volumes -> minimize the need of your local storage, only actively used data is stored -> stores data to S3
  - Tape Gateway - Virtual Tape Library -> archive data to cloud, virtual tapes to S3; directly to Glacier or Deep 

### Database
### Security, Identity & Compliance
### Network & Content Delivery

---

## 2017

### Compute
#### EC2 - Elastic Compute Cloud
##### EFS - Elastic File Storage - block based storage
##### Storage Gateway -

### Storage
#### S3 - Simple Storage System
- object based storage
#### Glacier - archiving solution

### Database
#### RDS - Relational Database Service
#### DynamoDB - NoSQL database
#### Redshift - data warehousing
#### VPC - Virtual Private Cloud

### Analytics
#### EMR
#### Kinesis
#### Cloud Search
#### Elastic Search

### Networking
#### Route 53 - DNS service
#### CloudFront - CDN
#### Direct Connect - connecting directly to AWS
##### Lambda - (not for exam)
- code will respond to events

### Security & Identity
#### IAM
#### Certificate Mgr
#### Directory Service

### Mgmt tools
#### Cloud Watch
#### Cloud Formation
#### Cloud Trail - auditing
#### Opsworks - Chef
#### Trusted Advisor
- automated environment scan

### Messaging
#### SNS - Simple Notification Service
#### SQS - Simple Queuing Service
#### SES - Simple Email Service

### Migration
#### Snowball - send your drives to AMZN
##### DMS - DB migration Service

### AWS
- Region is geographical area, each Region has at least 2 AZ (Availability Zones - can be considered a data center)
- Edge Locations are CDN end points for CloudFront

#### General information
- AWS 90% yoy growth, 15bln revenue for 2016 (approx)
- CS Arch Associate Exam covers: Messaging, Desktop (high level), Security & Identity, Network, Storage, Database, Mgmt tools, Compute
- Voice/ AI/ Machine Learning
    - _Polly_ text to speech
