# 1. SAA-C03

## 1. Cloud Computing Fundamentals

Cloud computing provides **on-demand access to computing resources**(servers, storage, databases and networking) over a network without owning the underlying infrastructure.

| Characteristic | Description | AWS Example |
|---|---|---|
| **1. On-Demand Self-Service** | Provision computing resources whenever needed without human interaction with the cloud provider. | Launch an EC2 instance in minutes. |
| **2. Broad Network Access** | Access cloud resources over networks using standard mechanisms. | Access AWS services through the AWS Console, CLI, or APIs. |
| **3. Resource Pooling** | Cloud providers share physical infrastructure among multiple customers while maintaining logical isolation. | Multiple customers run isolated EC2 instances on shared physical hardware. |
| **4. Rapid Elasticity** | Quickly scale computing resources up or down according to demand. | Use EC2 Auto Scaling to automatically adjust instance capacity. |
| **5. Measured Service** | Cloud providers measure resource consumption, enabling usage-based billing. | Pay for EC2 compute capacity based on usage. |

### 🎯 Key Concepts for AWS Certification

- **On-Demand Self-Service** → Provision resources whenever needed.
- **Broad Network Access** → Access resources over networks.
- **Resource Pooling** → Share underlying infrastructure securely.
- **Rapid Elasticity** → Scale resources up or down quickly.
- **Measured Service** → Measure consumption and pay based on usage.

> **Remember:** Cloud computing = On-Demand + Network Access + Resource Pooling + Elasticity + Measured Usage.

**Scalibility** is the ability to scale but **Elasticity** is the ability to adapt capacity to demand.
**Stateful** system require particular care when adding or removing capacity.
**Servless** lets you run code or consume services without provisioning or managing servers, servers still exist but the provider manages them (like AWS lambda).
**Availability**: Data and services are accessible when needed
**Durability**: Data survives failures
**High Availability** is to maximize uptime
**Scalibility** handles growth, **Elasticity** follows changing demand, **High Availibily** handles failure (do not minimize failure).

**RTO**: Recovery Time Objective : How quickly must we restore the service ? or How long can the service be unavailable ? **Acceptable Recovery Time**
**RPO**: Recovery Point Objective : How much recent data can we afford to lose, measured in time ? **Acceptable data loss**

### 1.1. Cloud Deployment Models — Public vs Private vs Multi-Cloud vs Hybrid Cloud

#### 🎯 Key Concepts for AWS Certification

- **Public Cloud** → Use infrastructure provided by a public cloud provider such as AWS.
- **Private Cloud** → Cloud infrastructure dedicated to one organization, hosted on-premises or by a third party.
- **Multi-Cloud** → Use multiple cloud providers.
- **Hybrid Cloud** → Integrate private and public cloud environments.

#### 💡 Practical Example — ELK Platform

| Model | ELK Deployment |
|---|---|
| **Public Cloud** | Deploy Elasticsearch, Logstash, and Kibana entirely on AWS EC2. |
| **Private Cloud** | Deploy the ELK stack on an organization's private cloud infrastructure. |
| **Multi-Cloud** | Deploy Elasticsearch on AWS and another application component on Azure. |
| **Hybrid Cloud** | Run Elasticsearch in an on-premises private cloud while running Logstash and Kibana on AWS, with secure connectivity between environments. |

> **Remember:** Public = Provider-managed infrastructure | Private = Dedicated cloud | Multi-Cloud = Multiple providers | Hybrid = Private + Public cloud.

### 1.2. Cloud Service Models — IaaS vs PaaS vs SaaS

- **IaaS (Infrastructure as a Service):** Provides virtualized computing resources (servers, storage, networking). You manage the OS, applications, and data.
  - **Example:** Amazon EC2.

- **PaaS (Platform as a Service):** Provides a managed platform to develop and deploy applications without managing the underlying infrastructure or OS.
  - **Example:** AWS Elastic Beanstalk.

- **SaaS (Software as a Service):** Provides ready-to-use software accessed over the internet. The provider manages the application and infrastructure.
  - **Example:** Gmail, Microsoft 365, Netflix ...

**🔑 AWS Certification — Remember:**
- **IaaS** → You manage OS + Applications + Data.
- **PaaS** → You manage Applications + Data.
- **SaaS** → You use the software; the provider manages the rest.

> **Key Difference:** IaaS = Infrastructure | PaaS = Development Platform | SaaS = Ready-to-use Software.

---

## 2. AWS-Fundamentals

### 2.1. Public vs Private Services


Refers to the networking only, not permissions.

- Public Internet: AWS is a public cloud platform and connected to the public internet. It is not on the public internet, but is next to it.
- AWS Public Zone: Attached to the Public Internet. S3 Bucket is hosted in the Public Zone, not all services are. Just because you connect to a public service, that does not mean you have permissions to access it.
- AWS Private Zone: No direct connectivity is allowed between the AWS Private Zone and the public cloud unless this is configured for that service. This is done by taking a part of the private service and projecting it into the AWS public zone which allows public internet to make inbound or outbound connections.

### 2.2. AWS Global Infrastructure

#### 2.2.1. Regions

AWS Region is an area that contains a full deployment of AWS infrastructure.
Each region contains multiple AZ

| Region Name | Region Code |
|---|---|
| US East (N. Virginia) | `us-east-1` |
| US West (Oregon) | `us-west-2` |
| Europe (Ireland) | `eu-west-1` |
| Europe (Frankfurt) | `eu-central-1` |
| Europe (Paris) | `eu-west-3` |
| Asia Pacific (Tokyo) | `ap-northeast-1` |
| Asia Pacific (Sydney) | `ap-southeast-2` |

**Why does AWS have multiple Regions?**
| Factor | Explanation |
|---|---|
| **Latency** | Deploy closer to users to reduce network delay. |
| **Compliance** | Meet legal data residency requirements. |
| **Service availability** | Not every AWS service or feature is available in every Region. |
| **Pricing** | AWS service prices can differ between Regions. |

📌 SAA-C03 Exam Tip: When choosing an AWS Region, evaluate latency, compliance, service availability, and pricing.

**AWS Regions are designed to be isolated from one another.**

AWS resources are generally regional.
Similarly, an S3 bucket is created in a specific region, although its name belongs to a global namespace.
Some AWS Services are global rather than Regional : IAM, Route 53, CloudFront

**Multi-AZ**:Protects against Availability Zone failures
**Multi-Region**: Can protect agaisnt complete regional failures.
**Region isolation**: A failure in one Region does not necessarily affect another
**Data residency**: Choose Regions that satisfy regulatory requirements
**Cross-Region replication**:	Maintain copies of data in another Region
**Failover**	Redirect workloads to healthy infrastructure

#### 2.2.2. AWS Edge Locations

Local distribution points. Useful for services such as Netflix so they can store data closer to customers for low latency high speed transfers.

Edge Locatiosn are primarily used for services such as :
- **Amazon CloudFront**: Content Delivery Network (CDN)>
- **Amazon Route 53**: DNS resolution through q globally distributed network.
- **AWS SHEILD**: DDoS protection integrated with AWS networking services.

              AWS REGION: PARIS
              ┌────────────────┐
              │                │
              │  S3 Bucket     │
              │  (Origin)      │
              │                │
              └───────┬────────┘
                      │
                      │
              Amazon CloudFront
                      │
          ┌───────────┴───────────┐
          │                       │
          ▼                       ▼
    Edge Location           Edge Location
       Tokyo                    Sydney
          │                       │
          ▼                       ▼
    Japanese Users          Australian Users

Let's understand what happens.

Scenario: A customer in Australia requests an image.

Step 1: The customer requests an image from your website.
Step 2: CloudFront routes the request to an appropriate edge location.
Step 3: CloudFront checks whether the requested image is already cached.

There are two possibilities:

Case A — Cache HIT ✅
The image is already cached at the edge location.
CloudFront returns the image directly to the customer.

Benefits:
- Lower latency.
- Faster content delivery.
- Fewer requests to the origin server.

Case B — Cache MISS ❌
The image is not available in the edge cache.
CloudFront retrieves it from the origin (potentially through a Regional Edge Cache), returns it to the customer, and caches it according to the configured caching behavior.

Future requests may then be served from the cache.

🎯 SAA-C03 — Remember
- **Cache HIT**: Content is served from the cache.
-**Cache MISS**: CloudFront must retrieve content from an upstream cache or the origin.
- **Origin**: The original source of content, such as S3 or an Application Load Balancer.
- **TTL** (Time to Live): Controls how long cached content remains fresh.
- **CloudFront**: Reduces latency and origin load through caching.
-** Cache invalidation**: tells CloudFront to remove specified objects from its edge caches so that subsequent requests retrieve updated content.

#### 2.2.3. Region's 3 Benefits

- Geographical Separation
  - Useful for natural disasters
  - Provide isolated fault domain
  - Regions are 100% isolated
- Geopolitical Separation
  - Different laws change how things are accessed
  - Stability from political events
- Location Control
  - Tune architecture for performance
  - Duplicate infrastructure at closer points to customers

### 2.3. Regions and AZs

Region Name: Asia Pacific (Sydney)
Region Code: ap-southeast-2

AWS will provide between 2 and 6 AZs per region.
AZs are isolated compute, storage, networking, power, and facilities.
Components are allowed to distribute load and resilience by using multiple zones.

AZs are connected to each other with high speed redundant networks.

#### 2.3.1. Service Resilience

**Service resilience** is the ability of a service or application to withstand failures, recover from disruptions, and continue providing its intended functionality.

1. **Globally Resilient**: IAM or Route 53 or CloudFront. No way for them to go down. Data is replicated throughout multiple regions.
2. **Region Resilient**: Operate as separate services in each region. Generally replicate data to multiple AZs in that region. S3, DynamoDB, regionql RDS services
3. **AZ Resilient**: Run from a single AZ. It is possible for hardware to fail in an AZ and the service to keep running because of redundant equipment, but should not be relied on. EC2, EBS Volumes

**Architecture A — Single EC2 Instance**
       Paris Region
            |
           AZ-A
            |
           EC2
            |
       Application

❌ If the instance fails, the application becomes unavailable.
❌ If AZ-A fails, the application becomes unavailable.
❌ If the Paris Region fails, the application becomes unavailable.

---

**Architecture B — Multi-AZ**
           Paris Region
                |
       Application Load
           Balancer
                |
         ┌──────┴──────┐
         |             |
        AZ-A          AZ-B
         |             |
        EC2           EC2
         |             |
     Application   Application

✅ Can survive an EC2 instance failure.
✅ Can survive an Availability Zone failure.
❌ Cannot guarantee availability during a complete Regional outage.

---

**Architecture C — Multi-Region**
              Amazon Route 53
                    |
           ┌────────┴────────┐
           |                 |
       Paris Region     Frankfurt Region
           |                 |
       Multi-AZ           Multi-AZ
       Application        Application
           |                 |
        Database          Database

✅ Can survive instance failures.
✅ Can survive AZ failures.
✅ Can survive a Regional outage if cross-Region replication and failover are correctly configured.

#### Service Resilience — Key Concepts

| Concept | Description |
|---|---|
| **High Availability (HA)** | Minimizing downtime by keeping services available during failures. |
| **Fault Tolerance (FT)** | Continuing to operate despite component failures, ideally without interruption. |
| **Disaster Recovery (DR)** | Restoring services after a major failure or disaster. |

**SAA-C03 — Remember:**
- **High Availability:** Minimize downtime (e.g., Multi-AZ deployment).
- **Fault Tolerance:** Continue operating despite failures (e.g., redundant active components).
- **Disaster Recovery:** Recover from major disruptions (e.g., Multi-Region failover).
- **Multi-AZ:** Protects against Availability Zone failures.
- **Multi-Region:** Can protect against Regional failures.

#### 2.3.2. AWS Resilience: RTO and RPO

- **RTO**: Recovery Time Objective
- **RPO**: Recovery Point Objective

##### 2.3.2.1. RTO: Recovery Time Objective
RTO defines the maximum acceptable time to restore a service after a disruption.

##### 2.3.2.2. RPO: Recovery Time Objective
RPO defines the maximum acceptable amount of data loss, measured in time.

**🎯 SAA-C03 — Remember**
| Concept	| Question it answers |
|---|---|
| **RTO** |	How quickly must the application recover? |
| **RPO**	| How much data loss can the business tolerate? |

#### 2.3.3. AWS Disaster Recovery Strategies

| Strategy | Description | Cost | Recovery Speed |
|---|---|---|---|
| **Backup & Restore** | Restore infrastructure and data from backups after a disaster. | Lowest | Slowest |
| **Pilot Light** | Keep critical components running in the DR Region; launch and scale the remaining infrastructure during recovery. | Low | Moderate |
| **Warm Standby** | Maintain a smaller but functional version of the application in another Region. | Higher | Fast |
| **Multi-Site Active/Active** | Run the application in multiple Regions simultaneously, serving production traffic. | Highest | Fastest |

**SAA-C03 — Remember:**
- **Backup & Restore:** Lowest cost, highest RTO/RPO.
- **Pilot Light:** Critical components are ready; remaining resources must be launched.
- **Warm Standby:** A reduced-capacity environment is already running and can be scaled up.
- **Active/Active:** Multiple Regions serve traffic simultaneously; potentially near-zero RTO and very low RPO.
- **Lower RTO/RPO generally means higher cost and complexity.**
- **RTO:** Maximum acceptable recovery time.
- **RPO:** Maximum acceptable data loss measured in time.

> **Important:** Actual RTO and RPO depend on the architecture, replication method, and failover configuration.

### 2.4. AWS Default VPC Virtual Private Cloud

VPC is a virtual network inside of AWS.
A VPC belongs to one region
A subnet is a smaller IP address range, belongs to one AZ, Configured to have one subnet in each AZ in the region by default.

A VPC is within 1 account and 1 region which makes it regionally resilient.
A VPC is private and isolated until decided otherwise.

One default VPC per region. Can have many custom VPCs which are all private by default.

Default VPC can be removed and recreated.

#### 2.4.1. Default VPC Facts

VPC CIDR(Classless Inter Domain Routing) - defines start and end ranges of the VPC.
IP CIDR of a default VPC is always: **172.31.0.0/16**

Custom VPCs can have many VPC CIDR but default VPC canhave only one.

Subnets are given one section of the IP ranges for the default service. 
They are configured to provide anything that is deployed inside those subnets with public IPv4 addresses. 

In general do not use the Default VPC in a region because it is not flexible.

Default VPC is large because it uses the /16 range.
A subnet is smaller such as /20
The higher the / number is, the smaller the grouping.

Two /17's will fit into a /16, sixteen /20 subnets can fit into one /16.

Three networking controls :
* subnets: where you place ressources
* route tables : where network traffic is directed 
* security groups : which traffic is allowed to reach or leave ressources

security groups are stateful, when they allow a connection, they automatically allow its response traffic
security groups applies to network interfaces such as EC2 isntance's interface instead NACL applies for subnet
security gps allow rules only, NACL allow and deny rules
security grps are stateful but NACL are stateless

### 2.5. Elastic Compute Cloud (EC2)

Default compute service. Provides access to virtual machines called instances.

#### 2.5.1. Infrastructure as as Service (IaaS)

The unit of consumption is an instance.
An EC2 instance is configured to launch into a single VPC subnet.
Private service by default, public access must be configured.
The VPC needs to support public access. If you use a custom VPC then you must handle the networking on your own.

EC2 deploys into one AZ. If it fails, the instance fails. **AZ Resilient**

Local on-host storage or **Elastic Block Storage**

Pricing based on:
- CPU
- Memory
- Storage **Persistent storage keeps data across restarts**
- Networking


#### 2.5.2. Running State

Charged for all four categories.

- Running on a physical host using CPU.
- Using memory even with no processing.
- OS and its data are stored on disk, which is allocated to you.
- Networking is always ready to transfer information.

#### 2.5.3. Stopped State

Charged for EBS storage  only.

- No CPU resources are being consumed
- No memory is being used
- Networking is not running
- Storage is allocated to the instance for the OS together with any applications.

#### 2.5.4. Terminated State

No charges, deletes the disk and prevents all future charges.

#### 2.5.5. AMI (Server Image) Amazon machine image

AMI can be used to create an instance or can be created from an instance.
AMIs in one region are not available from other regions.

Contains:

- Permissions: controls which accounts can and can't use the AMI.

  - Public - Anyone can launch it.

  - Owner - Implicit allow, only the owner can use it to spin up new instances

  - Explicit - Owner grants access to AMI for specific AWS accounts

- Root Volume: contains the **Boot Volume** that boots the OSs

- Block Device Mapping: links the volumes that the AMI has and how they're presented to the operating system. Determines which volume is a boot volume and which volume is a data volume.


#### 1.2.5.6. Connecting to EC2

AMI Types:

- Amazon Quick Start AMIs
- AWS Marketplace AMIs
- Community AMIs
- Private AMIs
- Windows using RDP (Remote Desktop Protocol), Port 3389
- Linux SSH protocol, Port 22

Login to the instance using an SSH key pair.
Private Key - Stored on local machine to initiate connection.
Public Key - AWS places this key on the instance.

### 2.6. S3 (Simple Storage Service) 

Global Storage platform. Runs from all regions and is a public service.
Can be accessed anywhere from the internet with an unlimited amount of users.

S3 is an object storage, not file, or block storage.
You can't mount an S3 Bucket.

block storage : provides a disk that an operating system can format and use (database files and elasticsearch data)

object storage : stores objects identified by keys inside containers called buckets ( backups, logs, images and archives)

file storage: provide a shared filesystem with files and directories (files shared by multiple machines)

#### 2.6.1. Objects

Can be thought of a file. Two main components:

- Object Key: File name in a bucket
- Value: Data or contents of the object
  - Zero bytes to 5 TB

Other components:

- Version ID
- Metadata
- Access Control
- Sub resources

#### 2.6.2. Buckets : a container for objects

- Created in a specific AWS Region.
- Data has a primary home region. Will not leave this region unless told.
- Unlimited number of Objects
- Name is globally unique
- All objects are stored within the bucket at the same level. **FLAT STRUCTURE**

If the objects name starts with a slash such as `/old/Koala1.jpg` the UI will present this as a folder. In actuality this is not true, there are no folders.

### 2.7. CloudFormation Basics
CloudFormation lets you define your infrastructure in code and deploy it automatically. this  is called IaC Infrastructure as Code.
CloudFormation templates can be used to create, update, modify, and delete infrastructure.

They can be written in YAML or JSON. An example is provided below.

```YAML
## This is not mandatory unless a description is added
AWSTemplateFormatVersion: "version date"

## If you use this section, it MUST immediately follow the AWSTemplateFormatVersion.
Description:
  A sample template

## Can control the command line UI. The bigger your template, the more likely
## this section is needed
Metadata:
  template metadata

## Prompt the user for more data. Name of something, size of instance,
## data validation
Parameters:
  set of parameters

## Another optional section. Allows lookup tables, not used often
Mappings:
  set of mappings

## Decision making in the template. Things will only occur if a condition is met.
## Step 1: create condition
## Step 2: use the condition to do something else in the template
Conditions:
  set of conditions

Transform:
  set of transforms

## The only mandatory field of this section **called logical resources**
Resources:
  set of resources

## Once the template is finished it can return data or information.
## Could return the admin or setup address of a word press blog.
Outputs:
  set of outputs
```

### 2.8. Resources

An example which creates an EC2 instance

```YAML
Resources:
  Instance: ## Logical Resource
    Type: 'AWS::EC2::Instance' ## This is what will be created
    Properties: ## Configure the resources in a particular way
      ImageId: !Ref LatestAmiId
      Instance Type: !Ref Instance Type
      KeyName: !Ref Keyname
```

Once a template is created, AWS will make a stack. This is a living and active representation of a template. One template can create infinite amount of stacks.
When creating a stack, while uploading template, what is actually doing is uploading the template directly to an S3 bucket that is created automatically.


For any **Logical Resources** in the stack,
CF will make a corresponding **Physical Resources** in your AWS account.

It is cloud formations job to keep the logical and physical resources in sync.

A template can be updated and then used to update the same stack.

deleting a stack => all logical ressources are deleted => all physical ressources are deleted

Change set	=> “What will CloudFormation change if I apply this update?”
Drift detection	 => “Does my deployed infrastructure differ from its expected configuration?”
