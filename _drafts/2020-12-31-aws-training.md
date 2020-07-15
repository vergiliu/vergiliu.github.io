---
layout: post
title: "AWS.training"
date: 2020-12-31 23:45:59
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
##### DynamoDB - document, scale horizontally
- read capacity unit (1read/sec for 4KB)
- write capacity unit (1write/sec for 1KB)
##### RDS - does not scale beyond single master
- read-replicas, aurora/mysql/mariadb/postgresql
##### ElastiCache - database caching
- memcached
- redis
- caching used strategically will improve performance
##### CloudFront - CDN/caching
- AWS Shield or WAF for protection