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
- we can create an Alarm in CloudWatch that triggers an event in the auto-scale group

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
- IOPS
    - can be provisioned
    - bursty for non-provisioned ones
- for pre-existing images for AMIs a volume is created from a snapshot of the OS image
- on AMIs termination they can be saved or discarded (default behavior)
- volumes w/ sizes between 1GB to 1TB can be created
    - create, attach (to instances)
    - volumes can have tags
- Snapshots
    - create from a running instance
    - for DB we should use a read replica, for which writes are temporarily suspended to improve snapshotting speeds
    - new volumes can be created directly from existing snapshots


ELB (Elastic Load Balancing)
- distributes traffic against multiple instances and multiple zones, automatically switching between all options
- works with VPC, you can create an internal LB environment for your internal network within the VPC
- integrated certificate management for SSL

IAM
- Groups
- Users
    - Permissions
- users login - https://<main-id>.signin.aws.amazon.com

Advanced topics
Auto Scaling
- the policy consits of
    - a launch configuration (AMI, instance type, security group) and
    - an auto scaling group (min/max numbers, AZ, ELB)
- the triggers can be done by CloudWatch, manually or on a schedule
- all costs are per hour, so one should pull up/down instances quickly
- exact params for as-launch are:
    - AMI image ID
    - Instance type
    - Key pair generated
    - Name of Security Group
    - launch config file
    - ec2-metadata -z (zone)
    - extra credentials are needed for authentication of the load balancer commands
    - create AS groups w/ `as-create-auto-scaling-group`
    - create launch config w/ `s-create-launch-config`
- auto-scaling instances are launched without names
    - can be tagged with `as-create-or-update-tags`
- AS will be integrated with ELB, and you can get notifications from SNS (Simple Notification Service) on changes
    - see details in SNS below
- we can create a scale up/down policy
    - as-put-scaling-policy lab-scale-up-policy --auto-scaling-group lab-as-group --adjustment=1 --type ChangeInCapacity --cooldown 300
    - as-put-scaling-policy lab-scale-down-policy --auto-scaling-group lab-as-group "--adjustment=-1" --type ChangeInCapacity --cooldown 300
- we can create an Alarm in CloudWatch that triggers an event in the auto-scale group
- all scaling can be analyzed by using `as-describe-scaling-activities --show-long`
- all autoscaling processes can be suspended/resumed w/ `as-supend/resume-...`


SNS
- create topic / email / create subscription - on ARN
- Topic ARN is necessary to manage it from the CLI tools
- to see all notifications `as-describe-notification-configurations`
- to update notifications for this LB group `as-put-notificaation-configuration`
    - arn:aws:sns:us-east-1:22549:lab-as-topic
    - as-put-notification-configuration lab-as-group --topic-arn arn:aws:sns:us-east-1:22549:lab-as-topic --notification-types autoscaling:EC2_INSTANCE_LAUNCH, autoscaling:EC2_INSTANCE_TERMINATE
