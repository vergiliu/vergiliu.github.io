---
layout: post
title: "AWS #1"
date: 2015-12-31 23:45:00
categories: [article, tutorial, noob]
tags: [tutorial, 2015]
---

`AWS`

### EC2
- all instances have a private + public IP (NAT-ed)
- Load Balancers
    - use the security group assigned when the instance was created
    - thresholds can be easily set/configured, as well as the "ping-path"
    - tags are supported
    - all metrics are automatically reported to cloudwatch
- Elastic IPs
    - can be assigned to running instances: they will be static IPs and can be assigned to one instance, work across availability zones
- instances tags
- in SecurityGroups of EC2 you can set IPs as instance ID
- EC2 security guide - http://aws.amazon.com/articles/Amazon-EC2/9001172542712674
- EC2 root device guide - http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html
- instances can have termination protection in order to prevent accidental deletion

### CloudWatch
- monitoring (ELB) dashboard
- details about zones, and request types
- we can create an Alarm in CloudWatch that triggers an event in the auto-scale group

### S3 (Simple Storage System)
- secure, durable, highly scalable object storage
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

### EBS (Elastic Block Store)
- persistent network attached storage, independent from the instance itself
- automatically replicated in the same Availability Zone (AZ)
- snapshotting is possible, with the snapshots saved in S3
- IOPS
    - can be provisioned
    - bursty for non-provisioned ones (default)
- for pre-existing images for AMIs a volume is created from a snapshot of the OS image
- on AMIs termination they can be saved or discarded (default behavior)
- volumes w/ sizes between 1GB to 1TB can be created
    - create, attach (to instances)
    - volumes can have tags
- Snapshots
    - create from a running instance
    - for DB we should use a read replica, for which writes are temporarily suspended to improve snapshotting speeds
    - new volumes can be created directly from existing snapshots

### EMR (Elastic Map-Reduce) - intro youtube - https://www.youtube.com/watch?v=Hhj3fOdt7zo
- process large amounts of data w/o managing instances
- launch & scale Hadoop applications using EC2 & S3
    - Apache Hive, Pig, Pig Latin support, most known Hadoop apps are directly available
- EC2 images are preconfigured
- Master the node which distributes jobs to other nodes
    - worker nodes
- Metrics about jobs can be viewed in CloudWatch
- cluster can be terminated on job completion
- wordSplitter.py for MR AWS file
- input and output (including logs) must be saved to S3, or be provisioned from S3
    - worker jobs can be as well from S3


### ELB (Elastic Load Balancing)
- distributes traffic against multiple instances and multiple zones, automatically switching between all options
- works with VPC, you can create an internal LB environment for your internal network within the VPC
- integrated certificate management for SSL

### EBS (Elastic Bean Stalk)
- deploy quickly web applications w/o knowledge of pieces required for cloud
    - monitoring, auto-scaling, load balancing all handled by EBS
    - no charge for EBS, you pay the resources that you're using
- various options to select the extras needed
    - web server - preconfigured options for most major platforms (Ruby, python, node.js, ...)
    - database server (RDS DB) - engine selection from MySQL, PostgreSQL, SQL Server, Oracle
        - instance type and database size
- next step is that all necessary AWS services are started and configured to support your environment

### IAM
- Groups
- Users
    - Permissions
- users login - https://<main-id>.signin.aws.amazon.com

### DynamoDB
- NoSQL, single-digit millisecond latency, document or key-value store

### SQS (Simple Queue Service)
- fully managed message queing service
- any level of data, at any throughput w/o losing anything


### SNS (Simple Notification Service)
- fully managed push notification service
    - push to devices, Windows, mobile, Baidu cloud, sms text, email, http to any endpoint
- create topic / email / create subscription - on ARN
- Topic ARN is necessary to manage it from the CLI tools
- to see all notifications `as-describe-notification-configurations`
- to update notifications for this LB group `as-put-notificaation-configuration`
    - we will need the identifier -> arn:aws:sns:us-east-1:22549:lab-as-topic
    - as-put-notification-configuration lab-as-group --topic-arn arn:aws:sns:us-east-1:22549:lab-as-topic --notification-types autoscaling:EC2_INSTANCE_LAUNCH, autoscaling:EC2_INSTANCE_TERMINATE

### CloudFormation
- create a stack of applications, from a JSON file that we can store in S3
    - collection of related AWS resources (stack), provisioning and updating them
    - QueueWatcherCount can be set to >0, so we can monitor the actions done by CloudFormation
    - Events lower tab we can see the progress of the current actions/operations
- Resources tab in the lower part shows the queues it uses/publishes to and policies
- Sample templates depending on AZ - http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-sample-templates.html

### SQS (Simple Queue Service)
- in actions of a particular queue we can see or delete messages (View/Delete messages), but while we're doing this no other applications have access to this queue
- various services (can) push messages to SQS/SNS so we can use them to register or track actions in AWS
- push notifications from SNS to SQS to be stored, later retrieved or analyzed

### VPC (Virtual Private Cloud)
- can create logically isolated AWS networking section w/ servers w/ or w/o internet access, own set of IP addresses and ranges
- when using the VPC wizard to create a VPC, we can use a NAT host for NAT-ing
- when launching EC2 instances we can assign them automatically to internal IP range and VPC
- best practice - mirror environment across 2 zones and add ELB to balance between them
- permits inbound and outbound rules for filtering traffic
    - Network ACLs are very expensive from a performance point of view
    - rules can be added to restrict also security groups
- creation of route tables and subnets can be performed from the left hand menus
    - by default the subnets don't have internet acces, safety first principle
- in SecurityGroups of EC2 you can set IPs as instance ID
- Bastion servers sit between the public internet and the private VPC, act as entrance point to the VPC or private network
    - m1.small should serve all but the largest networks, also no SSH-ing into this instance, on error just replace
- JSON
    - Mappings section; allows you to re-use the same template in all AZ, selecting only the required server types
    - VPC section: define the VPC address
        - here you need to specify the InternetGateway which will allow internet connectivity, AttachGateway
        - next specify the subnets (Type: AWS::EC2::Subnet), by referring (Ref) to the VPC we defined previously
        - routing tables (AWS::EC2::RouteTable): one for the public and one for the private networks, there can be up to 1 per subnet
            - we will specify the NAT server as router for the private section
            - next, we associate the subnets w/ the routing tables
        - next create Network ACLs (::PublicSubnetAcl and ::NetworkAcl) and then add the rules for ACLs
            - one pair ingress + egress, at least one pair needed
        - associate ACLs w/ subnets
    - create a NAT server (is a virtual EC2 appliance, we specified before)
        - to describe it we can specify REFerences to the Mappings we described in the JSON, e.g. Fn::FindInMap
        - NAT needs public (Elastic IP) to communicate w/ the Internet
        - define a NAT security group in order to specify which ports are opened or closed
        - also a private security group for communication inside the private Network
    - add an Output section
- to delete the stack we need to DISABLE termination protection in the EC2 for the NAT instance

### Advanced topics
#### Auto Scaling
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
    - `ec2-metadata -z` (zone)
        - lots of other informations can be gathered using this tool
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
- quick reference card for Auto Scaling - http://awsdocs.s3.amazonaws.com/AutoScaling/latest/as-qrc.pdf

#### Dynamic Registration
- Elastic environment - is highly utilized all the time, by using just in time provisioning
    - add instances as some metric increases (e.g. CPU), remove them when decreasing
- Scalable architecture - on-demand elastic growth w/o changing of architecture
    - Horizontal scaling: add/remove same size instances
- Bootstraping: automatically configure the instance on boot
    - by accessing already available and accessible meta-data
    - one such tool is CloudInit from Canonical/Ubuntu
    - CloudFormation: create and delete AWS (instances, AS groups, DynamoDB, etc) called stacks by using a JSON template
        - stacks can be used to create/update existing stacks
