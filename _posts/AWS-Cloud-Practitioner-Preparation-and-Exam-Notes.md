## Step 1

To start, try to read whitepapers to know more about AWS infrastructure. Read-only from AWS whitepapers page https://aws.amazon.com/whitepapers/.

This exam will be about these domains:

| Domain                  | Domain % of Examination |
| ----------------------- | ----------------------- |
| Cloud Concepts          | 26%                     |
| Security and Compliance | 25%                     |
| Technology              | 33%                     |
| Billing and Pricing     | 16%                     |

Focus on services in the following areas:

1. AWS shared responsibility model
2. AWS Cost Management
3. Security, Identity & Compliance: IAM; compliance concepts
4. Migration & Transfer
5. Network & Content Delivery: Virtual Private Cloud (VPC); Route53
6. Compute: EC2; Lambda
7. Storage: S3; Glacier
8. Databases: Relational Database Service (RDS); DynamoDB (Non-Relational Databases)
9. AWS Global Infrastructure

## Step 2: Cloud Computing

**Cloud Computing**: Cloud Computing is the on-demand delivery of computing, database, storage, application, and other IT resources under the label as a service via the internet with pay-as-you-go pricing.

## Step 3: 5 Pillars of Well-Architected Framework

These pillars help Architects build the most secure, fault-resilient, efficient, and high-performing IT infrastructure. Consider it the Best Practice framework of your Cloud Infrastructure.

- **Cost Optimization**: avoiding unnecessary costs
- **Reliability**: ability to prevent and quickly recover from operational failures
- **Operational Excellence**: daily system operations, monitoring, and improvements
- **Performance Efficiency**: using computing resources efficiently
- **Security**: protect information and systems

## Step 4: 6 Advantages of Cloud Computing

1. **Trade capital expense for variable expense**: you pay only when and what you consume
2. **Benefit from massive economies of scale**: you'll never be able to buy as much as Amazon
3. **Stop guessing about capacity:** access as much or as little capacity as you need with flexible scaling
4. **Increase speed and agility**: resources can be deployed or managed in minutes
5. **Stop spending money running and maintaining data centers**: no more physical infrastructures to maintain
6. **Go global in minutes**: deploy an application in multiple regions around the world with few clicks

## Step 5: Types of Cloud Computing

1. Infrastructure as a Service (IaaS) – you manage the provided hardware and software
2. Platform as a Service (PaaS) – Someone else manages the hardware, and you create/ deploy apps 
3. Software as  service (SaaS) – you use services

## Step 6: Types of Deployments

1. Public Cloud
2. Private Cloud
3. Hybrid – a mix of public and private

## Step 7: AWS Shared Responsibility Model

AWS and the customer share responsibilities for security and compliance of AWS. Lock Your Door!

- AWS is responsible for the security OF the cloud: hardware, software, networking, and facilities that help run the AWS Cloud.
  - Some services under AWS's responsibility to secure are Compute, Storage, Database, Networking, and global infrastructures such as Regions, Availability Zones, and Edge Locations.

- The customer is responsible for security IN the cloud: determined by the used services and the amount of configuration to keep secure systems.
  - Customer responsibility is data, OS, network, firewall configuration, client-side data, encryption, data integrity, and server-side encryption. IAM is an important part as well.

## Step 7: Security Pillars

* "The ability to protect the information, systems, and assets while delivering business value through risk assessments and mitigation strategies."*

There are 5 pillars in this concept:

- Detective Controls
- IAM
- Infrastructure Protection
- Data Protection
- Incident Response

[Pillar of AWS Well-Architected Framework](https://d1.awsstatic.com/whitepapers/architecture/AWS-Security-Pillar.pdf)

## Step 8: Compliance On AWS

Compliance is about:

- Certifications and attestations
- Laws, regulations, privacy
- Alignments and frameworks

The list on https://aws.amazon.com/compliance/ is the programs of compliance like ISO270001 and PCI DSS Level.

While AWS is compliant with some of these global services, it doesn't mean that you will automatically be compliant while using AWS.

You may AWS service instances get audited to make sure that they have the proper security measures in place.

## Step 9: IAM Identity Access Management

Global. Not an Id Management service. 

Provide very granular access permissions to resources within the AWS infrastructure.

Used to assign permissions to users or federated users. It controls access resources. It helps to create users, groups, and policies.

In general, interactions pass by Web Console, CLI, SDK.

**The Root Account:** The root account is based on the email. Root has full admin access. AWS advises to create users and attach roles, and secure the root account using multi-factor authentication.

**Groups:** a logic store to group users. Users in a group will inherit all permissions of the group. Setting permissions in a group means applying policies to the group.

**Policies:** a way to assign permissions to users, groups, and roles. Defined using JSON in a key-value pair. It starts with a version, then the statement.

**Roles:** more secure than access secrets, grant specific timed privilege. Assumed by IAM users, apps, or AWS services. Roles are global.

**Auth:** Login+password, Access key id + Secret access key, Session token.

`users/groups/roles ---authentication--> resource <--authorization--- policies`

## Step 10: WAF Web Application Firewall

Protects web applications from common web exploits to prevent app unavailability, compromise security, consume resources. It inspects the traffic coming into your application at layer 7. 

It is placed in front of a firewall and will determine what goes through a load balancer and its EC2 web servers or origin servers or CloudFront as part of your CDN configuration.

Maintain and create the rules using the AWS Management Console or API.

Pay for what you use, and the pricing is based on how many rules are deployed and how many requests the web application receives.

## Step 11: Managed DDoS Protection

**Shield**: Managed DDoS mitigation service. Protect web applications from DDoS attacks with an always-on detection and automatic handling of any potential DDoS attacks.

Global on all CloudFront and Route 53 Edge Locations. As a result, it protects web applications hosted anywhere globally by deploying CloudFront in front of them.

The origin servers can be S3, EC2, ELB, or custom servers not part of AWS.

Two tiers of AWS Shield:

- Standard - Free. Comes with all AWS accounts. Defends against most common network and transport layer DDoS attacks.
- Advanced - per month fees. For higher-level protection. Integration with WAF. (after DDoS attack, AWS refunds related charges Route 53, CloudFront, and ELB DDoS)

## Step 12: Inspector: Automated Security Assessment

Automated security assessment service to improve security and compliance of apps. It installs an agent on the EC2 OS. Checks run for vulnerabilities or deviations from best practices. 

It can provide an assessment at each point of your deployment cycle, not just on the production.

It produces a detailed list of security findings prioritized by level of severity. Results can be reviewed directly or as part of detailed assessment reports (via the console or API).

## Step 13: AWS Trusted Advisor

Global service for Business and Enterprise Companies. It to do security checks, reduce cost, increase performance. 

It provides real-time guidance to provision resources based on [best practices](https://aws.amazon.com/premiumsupport/trustedadvisor/best-practices/). Advisor advises on Cost Optimization, Performance, Security to Fault Tolerance. He will look at the entire AWS environment and provide a report detailing its checks and recommendations.

For infrastructure, he uses 5 Pillars of Well-Architected Framework. The status check is shown in 3 colors: green (no problem), yellow (investigation recommended), and red (action recommended).

Access to 7 core Trusted Advisor checks is available to all customers by default for free.

- S3 Bucket permissions
- Security Groups – Specific ports unrestricted
- IAM use
- MFA on Root Account
- EBS public snapshots
- RDS public snapshots
- Service Limits

## Step 14: Inspector vs. Trusted Advisor

Inspector is used to testing the security state of applications running on EC2 only (whereas Trusted Advisor can scan for vulnerabilities for many components of AWS infrastructure and the AWS account.)

## Step 15: EFS Elastic File System

Fully managed service. mAZs. Provides low latencies and high throughputs

Automatically grows and shrinks as storage demands go up and down.

Used by many EC2 as shared volume. 

Mountable in VPC through NFSv4 Protocol: 

`VPC[Subnet [ec2] + mount target] --> EFS`

Use to migrate data from on-premise to cloud over DirectConnect: 

`[corp]<----DirectConnect---->[aws [EFS Mount Target]]`

Options: 

- FSx Lustre >>> managed FS for intensive compute; non replicated; scale up/down; low latency and on-demand; integrate S3 for dataset; pay resources only and not hardware.
- FSx for Windows file server, support windows FS and SMB

## Step 16: EC2 Elastic Compute Cloud

VM, secure, scale-up/down behind an LB; boot in minutes, charging for used capacity. **(+)**: Web, DB, Server; Linux or Windows; Customizable hardware; Region; Capex and Opex; Design for failure (Have one EC2 instance in each AZ). SSH or a private key to access instances.

[opex; operating expense = daily expense to function biz] [capex; capital expense = incurred expense to create benefit]

Security Groups are virtual firewalls in the cloud.

**Types**: General purpose, compute optimized, memory-optimized, accelerated computing, storage optimized. 

- T2 is the most economical and commonly used instance.
- F [FPGA or Field Programmable Gateway] ; I [IOPS] ; G [Graphics] ; H [High Disk Throughput] ; T [cheap general purpose (T2 Micro)] ; D [Density] ; R [RAM] ; M [General purpose] ; C [Compute optimized] ; P [Graphics (think Pics)] ; X [Extreme Memory] ; Z [Extreme Memory AND CPU]

**Pricing**

- **On Demand**: Short-term or unpredictable workloads; cannot be interrupted; Fixed-rate by the hour (Win) or second (Linux), no commitment. 
- **Reserved Instances**: for stable computing; discountable reserved capacity. Upfront payments to reduce costs. 1 year or 3 years contract. 
  - Standard: edit type, size, network, etc. [can be sold]; Up to 75% discount
  - Convertible: Can change attributes as long as the resulting RI is equal or greater value [can not be sold]; Up to 54% discount
  - Scheduled RI: reserved time window; predictable, recurring schedule 
- **Spot Instances**: Apps that have flexible start and end times or urgent need of capacity; Bid on price for the instance capacity. Termination charges: partial by amazon, full charges by you.
- **Dedicated Hosts**: Regulatory requirements that don't allow multi-tenant virtualization. Physical server. Can reduce costs by using software licenses. 
- **Per second pricing**: for dev, test. 

**Placement Groups** 

Logical groupings or clusters of instances in the selected AWS region. It must be unique. Must have homogeneous instance types. Can't merge.

- Cluster Placement Group: grouping of EC2 instances inside a single AZ.
- Spread Placement Group: grouping of EC2 instances each on distinct and separate hardware.

## Step 17: AMI Amazon Machine Image

based on Vx. Image = {os + patches  + app} ; source = {AWS, MKP, Share, Creation}; Use pre-bult AMI or create custom AMI. 

Virtualization Types: Paravirtual (PV), Hardware Virtual Machine (HVM) 

- PV AMI: run on a special bootloader on the host, no hardware access;  
- HVM AMI: fully virtualized, run on OS directly, hardware access; 

**IP address**: {private, public, elastic}

**Access**: use key pairs at creation

## Step 18: Elastic Beanstalk: Easily Deploy/Scale

Compute service. Free. A quick and simple way to deploy, monitor, and scale apps by automating deployment.

Set settings, upload code, and Elastic Beanstalk handle everything else, like load balancing, provisioning, auto-scaling, and app health monitoring.


Focus on writing code.

Elastic Beanstalk keeps the platform running with the latest patches and updates.

Also, retain full control of the resources, so it is possible to change them around at any time.

## Step 19: ELB Elastic Load Balancing

It "balances" incoming application traffic "loads" across multiple "targets". 

It monitors the health; ensure to target healthy points.

Targets can be EC2, containers, IP, and can handle varying loads of traffic in a sAZs or mAZs.

Types:

- Classic: Slowly being phased out. Can do cross-zone load balancing
- App: app layer, 7th, of OSI model; use rule listener; evaluate rule and prioritize target; the route by path or host; redirect; register lambda as a target; Use CLoudWatch and CloudTrail to log and request tracing;
- Net: net layer, 4th, of OSI model; try to open a TCP connection to the target; very powerful

Tutorial

1 -  Create EC2 and run this script

```sh
##!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig on
cd /var/www/html
echo "Hello world. This is webserver01"> index.html
```

2 -  Browse to the public IP of this server then you should see the HTML displayed.

3 - Add more EC2 instances to the load balancer in different AZs, using the Target Groups.menu.

4- Use the DNS name on the load balancer to go to the web server via the load balancer.

## Step 20: Lambda

Lambda is a serverless compute service. 

Serverless means that the providers will provision and manage compute resources to run code. 

In Lambda, code runs in response to events and automatically scale on-demand.

Lambda extends AWS services and enables additional back-end services that operate at scale.

Try some [pre-built Functions](https://docs.aws.amazon.com/lambda/latest/dg/get-started-create-function.html) for commonly triggered events for even quicker set up.

You can create and test Lambda- based applications via Lambda Console, AWS CLI, or SAM CLI.

Blocks of Lambda

- **Lambda Function**: Custom code and any dependent libraries
- **Event Source**: AWS Service that triggers the function
- **Downstream Resource**: AWS Service that the function calls when triggered
- **Log Stream**: Annotate function code with custom logging statements for analysis. Invocations and metrics are automatically reported via CloudWatch.
- **Serverless Application Model**: Supported by CloudFormation and defines a simplified syntax for serverless resources

## Step 21: EBS Elastic Block Storage

Persistent block storage volume for EC2, different from S3; Duplicated and replicated within its AZ. Once attached, it creates a file system and uses them like any block device.

(+): segmentation, encryption, snapshot

Types:

- SSD: GP2 (1Gb-16Tb/ 16K IOPS) + Provisioned IOPS (500Gb-15Tb/ 32K or 64K IOPS) [boot volumes].
- HDD: Throughput Optimized Storage (500Gb-15T/ 500 IOPS) + Cold HDD (500Gb-15T/ 250 IOPS) [can not be boot volumes].
- Mag: 1Gb-1Tb/ 40-200 IOPS [boot volumes].
- Snapshots to S3: Snapshot of data. $0.05/GB-month data stored.

## Step 22: S3 Simple Storage Service

**Basics**: Unlimited, secure, durable, highly-scalable object storage. Web interface and CLI tools. Accept 0 – 5TB files. Amazon guarantee 99.99% availability and 11 x 9s durability (eleven 9's of durability). Also tiering, versioning, encryption. 

**Buckets**: container of files (folders like an FS), universal namespace so unique globally, by region to meet compliance [Example bucket URL: s3-[region].amazonaws.com/{bucketName}]

A key-value object also contains version, metadata, sub-resources (Access Control Lists)

**Permissions**: object level: ACL; bucket level: Bucket policy; IAM level: IAM policy

**Data consistency**: can read after upload immediately (1s). Overwrite PUTS and DELETES can take some time to propagate across AZs (delay of the 30s).

**Tiers/Classes**

*Standard*

- D/11x9s, A/4x9s, SLA/3x9s, AZ>=3, latency in ms
- default class, low latency + high throughput, redundant, sustain the loss of 2 facilities concurrently

*IA (Infrequently Accessed)*

- D/11x9s, A/3x9s, SLA/2x9s, AZ>=3, min-size/128kb, min-durability/30d, retrieve/GB, latency in ms
- Usage: infrequent rapid access; lower fee than S3, high fees on retrieve; suitable for -128 KB file; File stays at least 30 days.

*One Zone – IA*

- D/11x9s, A/99.5s, SLA/2x9s, AZ>=1, min-size/128kb, min-durability/30d, retrieve/GB, latency in ms

*Intelligent Tiering*

- Uses IA and Z-IA and moves non accessed data for more than 30 days to IA or reverse without impact and fees; Only available in one availability zone.

*Glacier*

- D/11x9s, A/-, SLA/-, AZ>=3, min-size/40KN, min-durability/90d, retrieve/GB, latency in mn
- Low-cost storage, archives only, Retrieval goes from minutes to hours; 
- Models: Expedited (1..5 mn for parts), Standard (3..5 hr) or Bulk (5..12 hr for large parts);

*Glacier Deep Archive*

- D/11x9s, A/-, SLA/-, AZ>=3, min-size/40KN, min-durability/180d, retrieve/GB, latency in mn
- Lowest storage class, retrieval time of 12 hours; 7-10 years retention

*Reduced Redundancy Storage, RRR* 

- like standard, not recommended

**Charges**: Charges apply per size, PUT requests including the lifecycle, retrieval, deletes, tiering, data transfer, acceleration, and replication.

**Security**:

- Encryption: Client-Side Encryption (CSE) or Server Side Encryption (SSE). [SSE-S3: S3 Managed Keys, SSE-KMS: Key Management Service, SSE-C: Customer Provided Keys]

- Access Control: By default, all buckets and objects are set to be private. Access Control List: Bucket and File-Level. Bucket Policy: Bucket Level

**Create S3**: view globally but buckets belong to individual regions, uploads are private by default, automatic cross-region replication in another bucket, transfer acceleration is used to upload on edge location (disabled).

**Website**: Create bucket, edit public access settings, and make files public, apply s3 web hosting, and choose sub domain.

**Lifecyle**: Setup rules to tier objects to better manage costs --> action = [class transition, expiration and delete]

**Version**: keep, restore, delete; state = unrevisioned | versioning_enabled | versioning_suspended 

**Cross-region replication**: async replication in other AZ and keeps MD + ACL

**Transfer Acceleration**: between S3 and client, uses CF and Edge locations to speed up the transfer 

**Storage Gateway**: Uses all the service in S3 to share files, volumes (stored or cached mode), tapes

`On-premise app <--SGW--> S3 Glacier: hybrid solution (extend to the cloud NFS/SMB)`

## Step 23: Snowball: Data Migration

Solution to transport data, large amount of data, into AWS.

PB-scale data migration to transport data from on-premise environment into the AWS Cloud. The process costs the 1/5th of the cost of data transfer via high-speed internet.

Receive a physical device, and transfer your files onto it. Once completed, ship it back, and the data is transferred into S3. It is possible to encrypt data. The transit can be tracked via Amazon SNS, text messages, or on the AWS Management Console.

50TB or 80TB in US regions. 80TB fir the rest for the world.

**Snowball Edge**: 100TB  transport and compute solution. A "datacenter in a box" as Lambda functions and EC2 compute instances in the device. 

**Snowball Mobile**: Snowmobile is a 45 foot shipping container, pulled by a truck to the office. 100PB of data. 

**AWS Import/Export**: old service name. Release of Snowball in 2015 to replace this service.

## Step 24: Storage Gateway: Connect On-Prem with Cloud

Connects your On-Premise storage with AWS Cloud storage

Like Snowball but stays on premise, to cache files inside data center and replicates the files directly to S3 over SSL.

It is a VM to install on a on-premise datacenter host linked to AWS account.

Types are File Gateway, Volume Gateway (Stored and Cached), and Tape Gateway.

- File Gateway: create file share to associate with the S3 bucket. The share is then accessible by clients using NFS or SMB protocol. Use multi-part parallel uploads or byte-range downloads.
  `[S3] <-- [DC + internet + VPC] --> [Stprage Gateway -- SMB NFS -- Server]`
- Volume Gateway:  based on iSCSI block protocol. Uploaded as "virtual hard disks".  Not backing up individual files, but rather, in "blocks". The blocks can be asynchronously stored as Elastic Block Store snapshots. There are two types of Volume Gateways: Stored Volume (keeps the complete copy on-premise while sending up snapshots to S3) and Cached Volume (keeps the most recently read data on-prem and has the complete copy on S3).
- The Tape Gateway utilizes the Virtual Tape Library (VTL) interface.

So.

- **File Gateway**: backs up individual files to S3
- **Volume Gateway**: backs up virtual hard disks using EBS snapshots ("block-based storage")
  - **Stored Volume**: stores complete copy locally
  - **Cached Volume**: stores complete copy on S3
- **Tape Gateway**: backs up data using virtual tapes

## Step 25: Databases

Providing relational and non-relational database services. It is possible to operate your own database in EC2 and EBS. 

**RDS: Relational Database Service**

RDS utilizes Read Replicas to enhance performance. It replicates the database across mAZs for durability.  Automated backups are stored in S3.

Multi-AZ, Read replicas (So EC2 read from replicas at first)

Types: SQL Server, Oracle, MySQL Server, PostgreSQL, Aurora (Amazon's own), MariaDB

Primary DB and secondary DB story and how AWS auto-failover to the secondary when the primary is down.

Writes are replicated to the read replicas.

Can configure: writes to the primary + read from the replicas.

Pay On-Demand or Reserved Instances.

**Non-Relational Databases**

These types of store data without structured linking. This allows the database to hold a considerable amount of data.

Types: DynamoDB, ElastiCache, Neptune

**Online Transaction Processing (OLTP) vs. Online Analytics Processing (OLAP)**

OLTP runs different queries than OLAP insert or pulls up a row of data to a DB.

OLAP Pulls in large numbers of records.

**Data Warehousing**

Data warehousing is used for business intelligence to pull and analyze huge and complex data sets.

Types: Redshift.

## Step 26: Redshift: Data Warehouse

AWS solution. Fully managed, petabyte-scale data warehouse service. It is cheap, fast, secure, and scalable.

It can scale data up to petabytes, which enables companies to acquire new insights for their businesses.

The idea is to pull in huge and complex data sets and do computation without worrying about primary and secondary DB. 

10 times faster performance than other data warehouses.

Encryption is compliant with SOC1, SOC2, SOC3, PCI DSS Level 1 requirements, and FedRAMP, and HIPAA eligible with BAA available from AWS.

Pricing:

- On-Demand Pricing: no upfront costs; hourly rate based on type and number of nodes in the cluster
- Redshift Spectrum Pricing: run SQL queries directly against all data in S3; pay for the number of bytes scanned
- Reserved Instance Pricing: save up to 75% over On-Demand; commitment of 1 or 3-year terms

## Step 27: Amazon Aurora: RDS

AWS solution. Fully-managed. MySQL and PostgreSQL are compatible. 

It provides commercial-grade database performance and availability at 1/10th of the cost.

It's up to 5x faster than MySQL and 3x faster than PostgreSQL as it is secure, available, reliable.

Fully managed: It automates administration tasks like provisioning, patching, backups, and database set up.

It features an auto-scaling, distributed, fault-tolerant, self-healing storage system with up to 15 low-latency Read Replicas. It can also replicate data across 3 Availability Zones.

## Step 28: ElastiCache: Non-SQL DB

Speed up the performance of existing databases (frequent, identical queries).

An in-memory cache in the cloud, managed, instead of slower disk-based databases.

In-memory caching engines: Memcached, Redis

## Step 29: DynamoDB: Non-SQL DB

AWS solution. Fully-managed. 

Global Tables fully automates the replication of tables across Regions. This enables you to have a fully managed, multi-region, and multi-master database with quick local read and write performance.

DynamoDB is free to try up to 25GB/month of indexed data storage, 200 million requests/month, and 2.5 million stream requests/month from DynamoDB Streams.

You can also deploy Global Tables in up to 2 AWS Regions for free.

## Step 30: CloudFront

CDN. Cache objects. Distribute content based on the geographical locations of the user through Datacenters. Content can be dynamic, static, streaming, and interactive.

Optimized to work with AWS services (origins) like S3, EC2, Elastic Load Balancing, Route53, and non-AWS server, which stores the original versions files.

Time to live (TTL) is the duration of caching for seconds (24h). Clearing cached objects will be charged.

**Edge Location**: Locations to cache content. EL are separated from AWS Region/AZ. Closer to users. With read/write content capability.

160 PoP = 150 EL + 10 Regional Edge Cache (unpopular content evicted from EL).

**Distribution**: Name was given to a collection of Edge Locations for content to distribute.

**Distribution Types**: Web Distribution, RMTP (Realtime message protocol).

Disable CloudFront distribution before proceeding with the deletion.

## Step 31: VPC Virtual Private Cloud

Networking service. Create a private virtual network within the AWS cloud infrastructure, isolated from the rest of AWS.

Like a box on the cloud to instantiate some AWS services, which is also completely isolated from everyone else's boxes.

VPC Features: 

- Utilizes high availability of AWS Regions and Availability Zones
- **Subnet**: Used to divide VPCs, and allows it to span multiple AZs
- **Route Table**: Controls traffic going out of subnets
- **Internet Gateway**: Allows internet access from VPC
- **NAT Gateway**: Allows private subnet resources to access the internet
- **Network Access Control Lists (NACL)**: Stateless control access to subnets
- **Security Group**: Routes traffic to the instance and functions as a built-in firewall

VPC Wizard uses one of the common network setups to spin up a VPC with automatically configured subnets, IP ranges, route tables, and security groups.

## Step 32: Route53 Cloud DNS

Global, Named after Route 66 but used 53 as it is the used port.

Redirect traffic all around the world.

Domains are registered in AWS via Route 53. 

Buy a domain via R53 and make sure you have an S3 bucket with the same name.

Basic Functions: Domain Registration, DNS, Health Checking, Auto naming for service discovery.

DNS records are organized into Hosted Zones (Public, Private) that to be configured via APIs.

Pay as you go, and only pay for what you use.

## Step 33: CloudFormation Automated Provisioning

Used to model and set up your AWS resources.

Create a template of the needed AWS resources using JSON file descriptor, and CloudFormation will take care of creating and configuring all the resources.

Use the Template Designer to customize a template or create a new one from scratch.

Elastic Beanstalk and CloudFormation are free services, but the resources they provide aren't free.

Elastic Beanstalk is limited to some services, while CloudFormation can provision most AWS services and is programmable.

Elastic Beanstalk is for developers that want to get their code up and running in the cloud fast. CloudFormation is for someone who knows AWS inside and out.

## Step 34: CloudTrail Track Usage

Regional service. CloudTrail is enabled per region and AWS account. Logs can be consolidated into a single S3 bucket belonging to the paying account. Simplify compliance audits. Track and automatically respond to security threats.

1/ Turn on CloudTrail in the paying account, 2/ create a bucket policy that allows cross-account access, 3/ Turn on CloudTrail in the other accounts and use the bucket in the paying account.

Can review the logs using CloudTrail Event History.

## Step 35: CloudWatch 

Monitor AWS resources, as well as the running applications on AWS.

Checks service every 5 minutes by default, or 1-minute intervals by turning on detailed monitoring.

CloudWatch alarms help to trigger notifications.

It monitors 

- **Compute**: EC2 (CPU, Network, Disk), Autoscaling Groups, Elastic Load Balancers, Route53 Health Checks
- **Storage & Content Delivery**: EBS Volumes, Storage Gateways, CloudFront

Some other AWS services used in conjunction with it are Amazon SNS (Simple Notification Service), EC2 Auto Scaling, CloudTrail, and IAM.

## Step 36: AWS Billing



## Step 37: AWS Cost Management

Cost Management includes services to organize and track cost and usage data through consolidated billing and access planning, budgeting, and forecasts for pricing optimizations.

**Support Plans** 

- **Basic**: Free, general customer service for account and billing questions and forums

- **Developer**: +$29/month, primary contact for technical questions via Support Center, response within 12–24 hours

- **Business**: +$100/month, 24 x 7 support via phone and chat, 1-hour response to urgent support cases, AWS trusted advisor for optimizing your AWS infrastructure, Access to AWS support API for automating your support cases

- **Enterprise**: +$15,000/month, Business plan includes, Technical account manager,  Support Concierge providing billing and account analysis and assistance, access to Infrastructure Event Management for support launches and migrations, 15-minute response to critical support cases

**Billing Alerts and Alarms**

Alerts and alarms are used to notify usage reached a specific limit. It sends a notification automatically. It can be set up globally in the AWS account in the Billing & Cost Management Dashboard and region-specific in the CloudWatch service.

## Step 38: Types of Charges

There are 3 types of charges you will accrue from your AWS infrastructure:

- Compute
- Storage
- Data Out

How AWS Pricing Works Whitepaper outlines the pricing models of AWS.

Inbound or data transfer between AWS services within the same region is free. The more data you transfer, the less you pay per GB.

## Step 39: Free Services

Free usage tier is available for new AWS users to practice using selected services for up to one year. The following services are free:

- Amazon VPC (a virtual data center in the cloud)
- Elastic Beanstalk (but the resources provisioned aren't free)
- CloudFormation (but the resources provisioned aren't free)
- Identity Access Management (IAM))
- Auto Scaling
- Opsworks (but the resources provisioned aren't free)
- Consolidated Billing

## Step 40: Calculators

AWS provides tools to calculate the estimated costs using several features:

- AWS Simple Monthly Calculator (deprecation in-progress)
  - Hosted on static website https://calculator.s3.amazonaws.com/index.html
  - Configure several options to estimate the total monthly cost of resources hosted on AWS
- Pricing Calculator at https://calculator.aws/##/ by generating an estimate and add services directly to the estimate or create a group and add the services to the group
  - Example, get an estimate without knowing the technical details of all of the Amazon EC2 instance types.
- AWS Total Cost of Ownership (TCO) Calculator
  - Used to compare the cost for on-premise infrastructure versus hosted on AWS
  - Provides reports for export to present to C-level executives as a business case to move to the cloud
- AWS Cost Explorer
  - Analyze cost and usage data to identify trends, cost drivers, and detect anomalies
  - Easy-to-use interface lets you visualize, understand, and manage your AWS costs and usage over time. Used to explore costs after they have been incurred.
- AWS Budgets 
  - Gives the ability to set custom budgets to alert when usage exceeds (or is forecasted to exceed) the budgeted amount. Used to budget costs before they have been incurred

## Step 41: Pricing

Pay as you go (PAYG), Payless when you reserve, Pay even less per unit by using more, Pay even less as AWS grows, custom pricing.

Pricing Policies: 

- start early with cost optimization: create cost controls before growing; scaling up or down must not be an obstruction to grow
- maximize the power of flexibility: pay for running resource only
- use the right pricing model for the job: On-Demand, Dedicated, or Reserved

Drivers of cost are compute, storage, data outbound.

**Ec2**: Clock hours of Server Time + model + number of instances + load balancing type + detailed monitoring + auto scaling + usage of elastic IP Addresses + OS and packages

**Lambda**: 1M free requests per month and 400,000 GB-seconds free of compute time per month. After that, $0.20 per 1M requests or $0.0000166667 for every GB-second. Price varies based on the used memory (with larger memory).

**EBS**: Volumes (per GB), Snapshots (per GB), Data Transfers.

**S3**: Storage class, use of storage, number of requests.

**Glacier**: Storage + data retrieval.

**Snowball**: pay as you use a gigantic disk to move your data to the AWS cloud. 50TB: $200 and  80TB: $250 for jobs; the First 10 days are free; after that, it's $15 a day.

**RDS**: Clock hours, characteristics, number of instances, provisioned storage, e.g., how big in GBs, additional storage, requests, data transfers.

**DynamoDB**: Provisioned throughput (write) + Provisioned throughput (read) +Indexed data storage.

**CloudFront**: traffic distribution, number of requests, data transfer out.

## Step 42: Organizations & Consolidated Billing

**Organizations**

Global Service. An account management service to manage AWS accounts centrally.

Available in two feature sets: Consolidated billing only or All features (Full access).

1/ Invite accounts via email or username or create new accounts. 2/ Create Organizational Units.

3/ Add the policies to the OU and/or apply to the accounts within the Organization

```
[root] --> [OU] -- > [OU] --> [Account]
            |                     |
            |_[Policy]            |_[Policy]
            
The root account is the base account.
OU – Organisational Unit – policies can be applied here
AWS accounts – policies can be applied here
```

**Consolidated Billing**

One root paying account. 20 linked accounts only. To add more, you need to contact AWS as this is a soft limit. One bill per AWS account. Track charges and gets a volume pricing discount. Unused reserved instances for EC2 are applied across the group.

Billing alerts per individual account.

**Best Practices with AWS Organizations**

- Always enable MFA and complex password on the root account.
- The paying account should be used for billing purposes only. Do not deploy resources into the paying account.

## Step 43: Tagging and Resource Groups

Key-value pairs attached to AWS resources to metadata them. Tags can sometimes be inherited.

Resource Groups: group resources using one or more tags assigned to them. Use automation with resource groups with AWS Systems Manager (EC2).

- For EC2 – Public and Private IP Addresses
- For ELB – Port Configurations
- For RDS – Database Engine etc

Tag Editor: manage and find all the tagged resources. Create query-based groups.

## Step 44: AWS Global Infrastructure

Global Infrastructure is built to provide fast, flexible, reliable, scalable cloud environments. Every component is designed to support redundancy and reliability. 

**Availability Zone (AZ):** contains one or more data centers. Identified by af-north-1a.

**Region (R):** geographical area with two or more AZs. Identified by af-north-1.

**Point of Presence (PoP):** Edge Locations and Lambda@Edge to cache content.

**Local Zone:** Private Gov cloud.

**Ground Station:** Control satellite communications.

Queries start in Edge locations to find the resource then R -> AZ -> DC -> EC2 or S3.

For an up to date number of AZs, PoPs, and Rs, see the https://www.infrastructure.aws/ website.

## Step 45: Autoscaling

Use Launch configurations to create an auto-scaling group to make a fault tolerate website.

## Step 46: Elastic Beanstalk

Used to provision and deploy applications. All you do is upload your code.

## Step 47: AWS System Manager

Used to view and control infrastructure on AWS. It can also be used within AWS and on-premise. 

For example, running an EC2 fleet will install agents. The agent allows to connect to Systems Manager and run commands once and run across all instances like install, patch, uninstall software.

Integrates with CloudWatch to give a dashboard of the entire estate. AWS CloudWatch allows collecting more system-level metrics or system logs.

## Step 48: Athena

An interactive query service to query and analyze data in S3 using standard SQL. Serverless, pay per query/per TB scanned. Also, it generates business reports on data like AWS cost and usage reports.

## Step 49: Macie

Security based on Machine Learning and NLP to discover, classify and protect sensitive data stored in S3 (Uses AI to recognize if your S3 objects contain sensitive data such as PII (Personally Identifiable Information)). It can analyze CloudTrail logs. Great for PCI-DSS and preventing ID theft. Has dashboards, reporting, and alerts.

## Step 50: CloudWatch and AWS Config

**CloudWatch**: monitors performance of resources and applications. Works great with EC2. Metrics reported back from instances via CloudWatch are CPU, Network, Disk, Status Check. Can add some script to create custom metrics.

**AWS Config**: provides a detailed view of AWS resources' configuration and how they relate to each other in an account. It shows the history of changes over time.

Must know the differences between Step 20: CloudWatch and AWS Config and the different use cases for inspector, AWS Trusted Advisor, CloudWatch, AWS Config.

## Step 51: Global and On-Premise AWS Services

**Global**: 

- IAM
- Route53
- CloudFront
- SNS
- SES

S3 gives Global Views but is regional.

**On-Premise Services**

- CodeDeploy: deploys code and apps to EC2 instances and also to on-premise web servers
- **Opsworks**: Like ElasticBeanstalk, uses Chef for automated deployments, runs on EC2 instances and on prem web servers
- **IoT Greengrass**: Basically IoT and connects devices to AWS cloud

## Step 52: Architecting for the Cloud Best Practices

**Traditional Computing vs. Cloud Computing**

- Traditional computing purchase physical resources and install the software manually and use as Provisioned Resources.
- Cloud Computing is Global, available, and offers scalable Capacity, Built-in Security, Higher Level Managed Services.
- Architecting (refactor & re-architecture) for cost and efficiency for business needs.

**Design Principles:** 

- Scalability: The key is stateless applications (easy to scale) (stateful with sticky session), distributed load on multiple nodes. Scale Up (Adding RAM or CPU inside an individual VM). Scale-Out (Adding multiple VM behind an ELB).
- Disposable Resources: Infrastructure As Code and use bootstrapping scripts to provision ECE2, Golden Images to clone EC2, Containers.
- Automation: Serverless Management and Deployment (Codepipeline, CodeDeploy), Infrastructure Management, and Deployment (Elastic Beanstalk), Alarms and Events (CloudWatch alarms, Lambda scheduled events, Web Application Firewall).
- Loose Coupling: Well Defined Interfaces (GW), Service Discovery (EC2 use RDS through DNS and not IP can help to auto-failover), Asynchronous Integration (use queues like RabbitMQ SQS), Distributed Systems (Graceful Failure in Practice is to inform users and admins about the error).
- Services Not Servers: Managed Services, e.g., lambda, s3, route53, and Serverless Architecture.

**Databases**: Multi-AZ (spread across 3 AZ or more), scalable

- **Aurora**: Anti-Patterns - use No-SQL if you need for joins or complex transactions.
- **DynamoDB**: Anti-Patterns - consider storing the large files (image, audio, video) in S3
- **Redshift**: Anti-Patterns - Not meant for On Line Transaction Processing (OLTP)
- **Search (CloudSearch, Amazon ElasticSearch)**
- **Graph Databases (Amazon Neptune)**

**Managing Increasing Volumes of Data**

Build a Data Lake (architectural approach to store massive amounts of data available to be categorized, processed, analyzed, and consumed). Store in S3. No need for predefined schema and use Athena to run SQL queries.

**Removing Single Points of Failure**

- Sharding || Redundancy || Detect Failure || Durable Data Storage
- Automated Multi-Datacenter resiliency (by region)
- Fault Isolation and Traditional Horizontal Scaling

**Optimize For Cost**

Sizing || Elasticity || Use purchasing options for each need

**Caching**

Application Caching (Elasticache) and Edge Caching (CloudFront)

**Security**

- Reduce Privileged Access || Defense in Depth (web application firewalls)
- Share Security Responsibility with AWS
- Security as Code (Create golden environments and deployed using CloudFormation).
- Real-Time Auditing (CloudTrail)

## Last Step

This link provides in-depth Cheat Sheets: https://digitalcloud.training/certification-training/aws-certified-cloud-practitioner/

AWS provide practice voucher to  attempt a free practice exam worth 20$ and prepares you for final certification. Google it before.
