---
layout: post
title: "AWS (Solutions Architect Associate) by CloudGuru"
date: 2017-06-01 22:22:22
tags: [video, reviews, 2017]
rating: na
---

## 2019
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
- IAM settings are all global
- best practice is to not directly use root account/"god mode"
- roles - allow one  service to use another service e.g. EC2 instance can use S3
- new users have no permissions when created 
  - key and secret ID for programmatic access
  - user/password for console access, different than keys mentioned above
  - only visible once, make sure not to loose them
- password policies and rotation can be configured
- policies can be AWS or customer-managed

### Compute
### Storage
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
