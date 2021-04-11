---
layout: post
title: "AWS.training"
date: 2021-12-31 23:45:59
tags: [training, online, reviews]
rating: na
---
resilience ~85%
performance
security

65 questions / 130 minutes / guessing w/o penalty

- decoupling improves scalability and availability
- failure needs to be treated as a regular event that can happen at any time
- fault tolerance (no impact to user ) is a higher requirement than HA (degraded state/performance)
- Recovery Time/Point Objective (RTO/RPO - how much data)

To study:
- well architected framework
- autoscaling in console
- https://aws.amazon.com/autoscaling/faqs/


#### VPC
- allow and deny incoming and outgoing traffic
- ability to isolate and expose services
- lives inside a Region, spans multiple AZs
  - defines subnets, can be public or private
- for a subnet to be public we need to attach an IGW to the VPC
- if we want a private VPC to have access to the internet, we need to provide a NAT Gateway
- NACL controls access to VPC, stateless

### Amazon Web Services
##### Auto Scaling
- CloudWatch, ELB, Auto Scaling
- there is a `Launch configuration` for AMI ID, Instance type, key pair, user data
- min, max, desired number of instances; health check type
#### Storage
##### Elastic Block Store (EBS)
- snapshots / encryption / provisioned capacity / independent lifecycle than EC2 instance
- multiple volumes striped to create large volumes
- SSD: gp2 / io1 (provisioned IOPS) HDD: st1 (throughput) / sc1 (cold)
##### Elastic File System (EFS)
- NFS 4.0 and 4.1 / Petabyte capacity / shared storage
##### Simple Storage (S3)
- distributed system, OBJECT STORAGE
- eventual consistency for updates
- versioning / encryption for data at rest (-C, -KMS, -CME) and in-transit (HTTPS) / 
- regional availability, 11-9's durability
- all static content should go to S3 (images, videos, css, js)
- buckets are always tied to a region
- pricing: GB/month, transfer out per region, PUT|COPY|LIST|GET requests
  - free into S3, free from S3 to CloudFront
- S3 lifecycle policies -> S3 / S3 Infreq Access / Glacier
- storage solution for unstructured data 
###### Glacier
- backup and archive storage
- retrieval: expedited, standard, bulk
- lifecycle buckets to auto-move data to Glacier

#### CloudFormation
- describe infrastructure as JSON templates
- parameters are not recommended, mappings are

#### AWS Lambda
- stateless code

#### Databases
##### RedShift - data warehouse, structured database
Analytical instead of transactional
##### DynamoDB - document, scale horizontally, managed NoSQL
- read capacity unit (1read/sec for 4KB)
- write capacity unit (1write/sec for 1KB)
- automatically shard data
- limitless storage due to horizontal scaling
- massive read/writes, e.g. > 150K/second
- simple GET/PUT queries
##### RDS - does not scale beyond single master
- read-replicas, aurora/mysql/mariadb/postgresql
- high durability
- complex transactions or complex queries
##### ElastiCache - database caching
- memcached - multithead, low maintenance, horizontal scaling w/ easy discovery
- redis - support for data structures, atomic operations, pub/sub, read replicas, sharding cluster
- caching used strategically will improve performance
##### CloudFront - CDN/caching
- AWS Shield or WAF for protection
- static and dynamic content
  - dynamic content: using AWS infra for request/reply

#### Performant architectures
- choose performant storage and databases
  - offload all static data to S3 for web servers
  - upload data to S3: virtual hosted or path style, buckets are tied to a region
  - price: storage, transfer out, API requests
  - S3 lifecycle policies; objects are immutable, only replacing no updating
- apply caching to improve performance
  - auto scaling - register/deregister w/ LB
  - Launch templates can be used for launching new instances
  - multiple policies can be attached to an Auto-Scaling group
- design solutions for elasticity and scalability

#### Security 
- shared responsibility model
- principle of least priviledge
- think of roles like guest passes at an office, temporary credentials
- policies attached to resources, which defines which operations are allowed
- IAM (users created within the account) / Roles (temp identity used by EC2, Lambdas, external) / Federation (users from AD or other corporate credentials) with roles assigned in IAM / Wed Identity Federation (users from Amazon.com or other OpenID provider) roles assigned using Secure TOken Service (STS)
- further reading: AWS security best practices whitepaper, IAM best practices, When to use STS
- VPCs
  - they represent private IP address space
  - within VPCs SG and ACL
  - IGWs / VPGw/ NAT GW to isolate
  - routes for traffic direction
- route table should include route to GW
- both apply to port/protocol / IP
  - SG at instance level - explicitly allow / stateful / applied to network interfaces (ENI)
  - NACL - allow and deny / stateless / applied to subnets
  - SG can span AZs , not regions
  - SG is per region, so you need to create a new security group for each region you plan on having it
- traffic in/out VPC:
  - IGW - connecting to internet
  - VPGW - connect to VPN
  - AWS Direct Connect - dedicate connection to customer DC
  - VPC peering - VPCs to each other
  - NAT GW - outgoing internet traffic from subnets
- further reading: VPC fundamentals, Bastion Hosts, VPC user guide
- SECURING THE DATA TIER
- AT REST: audit log for S3, 
  - Server Side Encryption: SSE-S3 , SSE-KMS, SSE-C (client provided keys) 
  - Client side: CSE-KMS (client side w/ keys managed by KMS), CSE-C 
- IN TRANSIT: SSL / VPN / IPSec / Snowball
- KMS / CloudHSM
- further reading: Security overview whitepaper, IAM policies and bucket policies and ACLs
- axioms: lock down root users, SG only ALLOW!, NACL allow explicit DENY, prefer IAM to access keys

#### Cost optimization
- Pay as you go, reserved instances are cheaper, spot instances
- storage class, storage amount, how many and big volumes, snapshots, data transfer for moving to different regions
  - move to Infrequent Access (IA) or Glacier
  - EFS is block storage
- AXIOMS: if it's on reserve it, unused CPU is wasting money, use cost-effective storage and class, determine most cost effective EC2 instances

#### Operational Excellence
- operate it in an automated way, perform operations with code
- make frequent small, reversible changes
- refine operations and procedures frequently
- anticipate failure
- learn from all operational failures
- CloudFormation, Trusted Advisor, CloudTrail (logs API calls), VPC Flow logs (L3 and L4), Inspector (EC2 checks for vulns), Trusted Advisor (checks accounts)
- CloudWatch - metrics and alarms, logs storage log files, EC/CloudTrail/Lambda - can extract patterns from log lines
- secrets on ec2 instaces are an anti-pattern
- axioms: IAM roles safer than keys/passwords, provide alerts to anomalous conditions, monitor metrics, automate responses to metrics