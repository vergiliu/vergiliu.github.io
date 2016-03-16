---
layout: post
title: "AWS #1"
date: 2015-12-31 23:45:00
categories: [article, tutorial, noob]
tags: [tutorial, 2015]
---

`AWS`

EC2
- all instances have a private + public IP (NAT-ed)
- Load Balancers
    - use the security group assigned when the instance was created
    - thresholds can be easily set/configured, as well as the "ping-path"
    - tags are supported
    - all metrics are automatically reported to cloudwatch
- Elastic IPs
    - can be assigned to running instances: they will be static IPs and can be assigned to one instance, work across availability zones
- instances tags

CloudWatch
- monitoring (ELB) dashboard
- details about zones, and request types

S3 (Simple Storage System)
- charged is done based on storage and requests of data (in and out), not on bucket(s) creation
- name of bucket *must be unique across all S3 buckets* (really?!?)
- backup and storage, media and app hosting, high traffic hosting
- data is represented as Objects in Buckets
    - object = file + metadata
    - one or more buckets in the system
    - access control to buckets, access logs and regions can be configured/set
- Permissions can be set as JSON (not very convenient, but there are some examples)
- Static Website Hosting (endpoint - Endpoint: webapplication-site-9988.s3-website-us-east-1.amazonaws.com)
- Enable CORS (Cross-Origin Resource Sharing)
    - for JS, "wide array of configrations options"
- to be readable by everyone you will need to have Everyone - List as Add more permissions

EBS (Elastic Block Store)
- persisten network attached storage, independent from the instance itself
- automatically replicated in the same Availability Zone (AZ)
- snapshotting is possible, with the snapshots saved in S3
- IOPS - can be provisioned
    - bursty for non-provisioned ones

ELB (Elastic Load Balancing)
- distributes traffic against multiple instances and multiple zones, automatically switching between all options
- works with VPC, you can create an internal LB environment for your internal network within the VPC
- integrated certificate management for SSL

IAM
- Groups
- Users
    - Permissions
- users login - https://<main-id>.signin.aws.amazon.com



[iTunes]: https://itunes.apple.com/
[Goodreads]: https://www.goodreads.com/
[Play Store]: https://play.google.com/store/books/
[Books]: http://www.amazon.com/
