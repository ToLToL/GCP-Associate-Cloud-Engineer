# GCPAssociateCloudEngineer
Notes to pass the GCP associate cloud engineer certification

## Resources
1. [A Cloud Guru - GCP certified associate cloud engineer](https://learn.acloud.guru/course/gcp-certified-associate-cloud-engineer)
2. [GCP - AWS products mapping](https://cloud.google.com/free/docs/aws-azure-gcp-service-comparison)
3. [Every product in the Google Cloud family described in <=4 words](https://github.com/priyankavergadia/google-cloud-4-words)
4. [Exam topics - free pool of accurate test questions](https://www.examtopics.com/exams/google/associate-cloud-engineer/)
5. [Whizlabs - paid but you have a free test with 25 questions](https://www.whizlabs.com/google-cloud-certified-associate-cloud-engineer/)
6. [Sketch on GCP services by Priyanka, Cloud advocate at GCP](https://github.com/priyankavergadia/GCPSketchnote)
7. [Gcloud commands cheat sheet](https://cloud.google.com/sdk/docs/images/gcloud-cheat-sheet.pdf)


## Shortcuts

| Description | Command |
| --- | --- |
| Find products and services | `/` |
| Open shortcut help | `? or Control+Shift+/` |
| See all notifications/activity | `G then N` |
| Open help menu | `G then H` |
| Activate Cloud Shell | `G then S` |
| Open project navigator | `Control+O (Command+O on macOS)` |
| Go up one page | `U` |

## Introduction

### 1. Pricing

- Provisioned: "Make sure you can handle X"
- Usage: "Handle whatever I use and charge me for that"
- Network
  - Ingress: free
  - Egress: charged (sometimes free depending on destination service / location of that service)

### 2. Security

- Separation of dutiies / physical security
- Everything encrypted at rest
- Strong key / identity management
- Network encryption
- Distrust the network: BeyondCorp

### 3. Resource Quotas

- Scope
  - regional
  - global

- Changes
  - automatic
  - by request (response in 24 - 48h)

### 4. Organization

- Project = AWS account
- Project own ressources
- Resources can be shared with others projects
- Projects can be grouped et controlled in a hierarchy

## Account setup

### 1. Cloud Shell

- Web browser access
- Automatic SSH key management
- 5 GB of persistent storage
- Pre-authorized and always up-to-dat preinstalled tools (gcloud, kubectl, docker, pip/python, ruby, vim, node, bash ...)
- Web preview of running web app on local port

## Basic services

### 1. Cloud storage

Name must be unique across Cloud Storage.

#### Location Type
| Location Type | Description |
| --- | --- |
| `Multi-region` | Highest availability across largest area |
| `Dual-region` | High availability and low latency across 2 regions |
| `Region` | Lowest latency within a single region |

#### Storage class
| Storage class | Description |
| --- | --- |
| `Standard` | Best for short-term storage and frequently accessed data |
| `Nearline` | Best for backups and data accessed less than once a month |
| `Coldline` | Best for disaster recovery and data accessed less than once a quarter |
| `Archive` | Best for long-term digital preservation of data accessed less than once a year |

#### Access control
| Access control | Description |
| ----- | --- |
| `Uniform` | Ensure uniform access to all objects in the bucket by using only bucket-level permissions (IAM). This option becomes permanent after 90 days. |
| `Fine-grained` | Specify access to individual objects by using object-level permissions (ACLs) in addition to your bucket-level permissions (IAM) |


#### Protection tools

| Protection tools | Description |
| --- | --- |
| `Object versioning` | Best for recovery, restoring deleted or overwritten objects. To minimize the cost of storing versions, we recommend limiting the number of noncurrent versions per object and scheduling them to expire after a number of days. |
| `Retention policy` | Best for retention policy, preventing the deletion or modification of the bucket's objects for a specified minimum duration of time after being uploaded.  |

### 2. gcloud

- `gsutil`: gcloud storage
- `bq`: gcloud bigquery

In general console > gcloud > rest API

`curl -H "Metadata-Flavor:Google" metadata.google.internal/computeMetadata/v1/`: Compute Engine defines a set of default metadata entries that provide information about your virtual machine (VM) instance or project. Default metadata is always defined and set by the server.

### 3. Compute engine

**Preemptibility** (AWS EC2 Spot instance): a preemptible VM costs much less, but lasts only 24 hours. It can be terminated sooner due to system demands


## Scaling

### Instance group

- **managed instance group**: Single / multi-zone
- **unmanaged instance group**: Single-zone only. best for load balancing instances that we can add / remove arbitrarily. Autoscaling, autohealing and rolling updating are not supported


## Security (IAM)

### Breakdown

#### 1. Permission
A permission allows to perform a certain action in the form of: **Service.Resource.Verb**. It usually correspond to REST API methods.
example: pubsub.topics.publish

#### 2. Role
A role is a collection of permission to use GCP resources.

- Primivite role: project-level and often too broad:
  - Viewer, Editor, Owner
- Predefined roles: granular access to specific resources
  - roles/bigquery.dataEditor, roles/pubsub.subscriber
- Custom role: granular project or organization-level roles we define

#### 3. Members
Can be:

- user: Google acccount
- serviceAccount: apps / services
- group: group of users and service accounts
- domain: whole domain managed by G Suite or Cloud Identity
- allAuthenticatedUsers: any Google account or service account
- allUsers - Anyone on the Internet

A group has a unique email that is associated with it. Recommended.
example: 1 group for each department

#### 4. Policies
A policy binds members to roles for some resources.

Roles and members listed in policy, but resources are identified by attachment.
Always additive ("Allow") and never subtractive ("Deny"), child policies cannot restrict access granted at a higher level.

### AAA Data flow

1. Authentication - Who are you ?
  - Identity
    - G Suite, Cloud Idendity
    - Applications & service us Service Accounts
  - Identity hierarchy
    - Google groups
2. Authorization - What are you allowed to do ?
  - Identity hierarchy (Google groups)
  - Resource hierarchy (Organization, folders, projects)
  - IAM
    - Permissions
    - Roles
    - Bindings
  - GCS ACLs
  - Billing management
  - Networking structure & restrictions
3. Accounting - What did you do ?
  - Audit / activity logs (Stackdriver)
  - Billing export
    - BigQuery
    - File (GCS bucket) 


## Billing accounts

Lives outside of projects can inherits only Organization-level IAM policies, not the project ones

![Screenshot 2022-01-03 at 15 39 37](https://user-images.githubusercontent.com/39993930/147943413-0ee429f2-6396-4a29-8f98-06bbc92dfba6.png)
![Screenshot 2022-01-03 at 15 52 10](https://user-images.githubusercontent.com/39993930/147944846-e045ad48-23fe-4d93-8f8a-dff563be11dd.png)
![Screenshot 2022-01-03 at 15 53 34](https://user-images.githubusercontent.com/39993930/147945005-807bd83f-ef52-4438-9d67-933e561a7f8d.png)


## Networking

![Screenshot 2022-01-03 at 15 57 49](https://user-images.githubusercontent.com/39993930/147945553-e71b5ae4-beff-4d09-9075-a221194ac369.png)

### Routing schemes

#### Unicast
There's only one unique device that can handle the work, so send it there.

![Screenshot 2022-01-03 at 16 39 00](https://user-images.githubusercontent.com/39993930/147949898-ed14926a-17f3-4ce2-82eb-159680231044.png)

#### Anycast
There are multiple devices that can handle the work, so send it to any one.

![Screenshot 2022-01-03 at 16 39 45](https://user-images.githubusercontent.com/39993930/147949972-f86592a0-8c5f-422b-8a57-ff14fac678bf.png)

### Layer 4 vs Layer 7

#### Layer 4
TCP/IP: IP addresses

#### Layer 7
HTTP and HTTPS: URLs and paths

### Private address ranges

- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

### Firewall rules

#### Targets

- All instances in the VPC
- Service Account
- Specified tags


## Compute services

### GCE - Compute Engine (Regional)

Pay by the second.
Substained use discount: automatically cheaper if you keep running it.
Even cheaper for "preemptible (AWS spot instance) / long-term use commitment in a region (AWS reserved instance)".
Live migration: Google seamlessly moves instance across hosts.

### GKE - Kubernetes Engine (Regional)

Pay for underlying GCE instances.
No GKE management fee, no matter how many nodes we have.

### GAE - App Engine (Regional)

PaaS --> Heroku, AWS elastic beanstalk, take code and runs it.

Flex mode: can run any container
Auto-scales based on load.

### GCF - Cloud Functions (Regional)

FaaS -> AWS lamba

Pay for CPU / RAM assigned to function per 100ms
Each function gets an HTTP endpoint
Can be triggered by GCS objects, pub / sub message
Massively scalable horizontally

## Storage services

![Screenshot 2022-01-10 at 17 01 57](https://user-images.githubusercontent.com/39993930/148797514-ec9bad6d-4793-4af1-84c0-5b606775bde3.png)

### Local SSD (Zonal)

AWS EC2 store volumes - Direct-attached storage (DAS)
Data will be lost whenever the instance shuts down, but can survive Live Migration
Data encrypted at rest
Pay by GB / month provisioned

### PD - Persistent Disk (Zonal)

AWS Elastic Block Storage / Storage Area Network (SAN)

Block-based network-attached storage, boot disk for every GCE instance.
Persistent disks are replicated
Can resize while in use.
Snapshots
Can be mount to multiple nstances if all are read-only
Pay for GB / month provisioned depending on performance class + snapshot GB / month used

### Cloud Filestore (Zonal)

AWS EFS elastic file system - NAS Network-attached storage

Fully-managed file-based storage
Accessible to GCE and GKE through your VPC via NFSv3 protocol
Pay for provisioned TBs in:
- Standard (slow)
- Premium (fast)

### Cloud storage (Regional / multi regional)

AWS S3 - Glacier

Infinitely scalable, fully-managed, versioned and durable object storage
99.999999999 (eleven 9) durability
Site hosting / CDN functionality
Lifecycle transitions across classes:
- Multi-regional
- Regional
- Nearline (once / month)
- Coldline (once / year)

Pay for data operations & GB / month stored by class
Nearline / Coldline: also pay for GBs retrieved + early deletion

## Database services


### Cloud SQL (Regional)

AWS RDS

Fully-managed MySQL and PostgreSQL
Supports automatic replication / backup / failover
Scaling is manual
Pay for underlying GCE instances and PDs
Binary Logging enables Point in Recovery in Cloud SQL

### Cloud Spanner (Regional / Multi-regional / Global)

First horizontally scalable, strongly consistent, relational database service:
- from 1 to thousands of nodes
- Recommended: at least 3 nodes for prod

SLA: 99,999% for multi-region
Pay for provisioned node time + used storage-time

### BQ - BigQuery (Multi-regional)

AWS Redshift + AWS Athena

Serverless column-store data warehouse for analytics using SQL
Pay for GBs actualy considered (scanned) during queries:
- attempts to reuse cached results which are free
Pay for data stores GBs / month
- cheaper when table not modified for 90 days
Pay for GBs added via streaming inserts

### Cloud BigTable (Zonal)

AWS DynamoDB, Apache Hbase

Low latency & high throughput NoSQL Db for large analytical apps.
Supports HBase API
Integrates with Hadoop, Dataflow, Dataproc
Scales seamlessly
Pay for processing node hour, GB / hours used for storage

### Cloud Datastore (Regional / Multi-regional)

AWS DynamoDB

Managed & autoscaled NoSQL DB with indexes, queries and ACID.

### Firebase Realtime DB (Zonal) - Cloud Firestore (Multi-regional)

NoSQL documents stores with real-time client updates wia managed websockets
- Free tier (Spark)
- Flat tier (Flame)
- usage-based pricing (Blaze)

Realtime DB: pay more for storage and GB downloaded
Firestore: pay for operations and much less for storage and transfer


## Data transfer services


### Data transfer appliance

AWS Snowball

Physically ship data to GCS.
2 versions:
- 100 TB
- 480 TB

### Data transfer service (Global)

Copies objects for us, don't need to set up a machine.
Source: S3, HTTP / HTTPS endpoint, GCS bucket
Destination: GCS bucket
One-time or scheduled recurring transfers
Free to use but pay for its actions


## External networking

![Screenshot 2022-01-10 at 16 58 32](https://user-images.githubusercontent.com/39993930/148796963-ae9b0563-f163-49bc-8ad7-d5eb521ad998.png)

### Google domains (Global)

AWS Route 53

Google's registrat for DN.

### Cloud DNS (Global)

AWS Route 53

Managed DNS service
100% uptime guarantee
Public and private managed zones.

## Internal networking

### VPC - Virtual Private Cloud (Regional - Global)

Global IPv4 unicast Software-Defined Network (SDN) for GCP resources
Automatic / custom mode.
VPC is global, subnets are regional.

### Cloud Interconnect (Regional - Multi-Regional)

Connect external network to Google's.
Private connections to VPC via Cloud VPN or Dedicated/Partner Interconnect

### Cloud VPN (Regional)

IPsec VPN to connect to VPC via public internet for low-volume data connections.

### Dedicated Interconnect (Regional - Multi-Regional)

Direct physical link between VPC and on-prem data center for high-volume data connections

### Cloud Router (Regional)

Dynamic routing for hybrid networks linking GCP VPCs to external networks.
Works with Cloud VPN and Dedicated Interconnect

### Dedicated Interconnect (Regional - Multi-Regional)

Direct connectivity to CDN providers.

## Machine learning / AI

### Cloud Machine Learming Engine (Regional)

AWS SageMaker
Massively scalable managed service for training ML models & making predictions
Enable devs to user TensorFlow on datasets of any size
Integrates:
- Storage: GCS / BQ
- Dev: Cloud Datalab
- Preprocessing: Cloud Dataflow

### Cloud Vision API (Global)

AWS Rekognition
Classifies images into categories, detects objects / faces & finds / reads printed text
Pre-trained ML model to analyze images and discover their contents
Upload images or point to ones stored in GCS

### Cloud Speech API (Global)

Automatic speech recognition (ASR) to turn spoken work audio files into text.
Pre-trained ML model to recognizing speech in 110+ languages.

### Cloud Natural Language API (Global)

Analyzes text for sentiment, intent, content classification and extracts info.
Pre-trained ML model for understanding what text means.
Works with Speech API (audio), Vision API (OCR = optical character recognition), Translation API

### Cloud Translation API (Global)

Pre-trained ML model for recognizing and translating.

### Diagflow (Global)

AWS Lex
Build converstational interfaces for websites, mobile apps, messaging, IoT devices.
Pre-trained ML model and service for accepting, parsing, lexing input & responding.

### Cloud Video Intelligence API (Regional - Global)

Annotates videos in GCS with info about what they contain.
Pre-trained ML model for videos scene analysis and subject identification.
- Label dectection: detect entities: cat, flower
- Shot change detection
- SafeSearch detection: detect adult content

### Cloud Job Discovery (Global)

Helps career sites, company job boards to improve engagement and conversion
Pre-trained ML model to help job seekers search job posting databases.

## Big data & IoT

![Screenshot 2022-01-10 at 10 15 42](https://user-images.githubusercontent.com/39993930/148742077-741174ef-2933-42b5-83e1-8d6101cb3334.png)


### Cloud IoT Core (Global)

AWS IoT
Fully-managed service to connect, manage and ingest data from devices globally.
![Screenshot 2022-01-10 at 10 17 45](https://user-images.githubusercontent.com/39993930/148742345-208e0b98-16d9-4d68-9876-65dad44b5a9a.png)

### Cloud Pub / Sub (Global)

AWS SNS, SQS
Infinitely-scalable at least-once messaging for ingestion, decoupling.
Messages up to 10MB and stored for 7 days.


### Cloud Dataprep (Global)

AWS Glue
Visually explore, clean and prepare data for analysis without running servers.
Data wrangling for business analysts not IT pros.

### Cloud Dataproc (Zonal)

AWS EMR, Spark, Hadoop
MapReduce processing via configurable, managed Spark & Hadoop clusters.


### Cloud Dataflow (Zonal)

Smartly-autoscaled & fully-managed batch or stream MapReduce-like processing.

### Cloud Datalab (Regional)

Uses Jupyter Notebook, interactive tool for data exploration, analysis, visualization and machine learning.

### Cloud Data Studio (Global)

Big Data visualization tool for dashboards and reporting

### Cloud Genomics (Global)

Store and process genomes and related experiments

## Identity & access

### Roles (Global)

AWS IAM policies
Roles are collections of Permissions to use / manage GCP resources.
Permissions allow you to perform certain actions: `Service.Resource.Verb`
- Primitive Roles:
  - Owner (control access & billing)
  - Editor
  - Viewer

- Predefined Roles: granular access to specific GCP resources
e.g.: `roles/bigquery.dataEditor`
- Custom Roles

### Cloud IAM (Global)

AWS IAM
Controls access to GCP resources: authorization.
Member is:
- user
- group
- domain
- service account: belongs to application / instance

Every identity had a unique e-mail address, including service accounts
Policies bind Members to Roles at a hierarchy level: Org, Folder, Project, Resource

![Screenshot 2022-01-10 at 10 38 58](https://user-images.githubusercontent.com/39993930/148744857-7ef4e93d-00c7-467b-82b8-235f649285d2.png)


### Service accounts (Global)

AWS IAM Roles
Special type of Google account that represents an application not an end user
Should use service account rather than user accounts / API keys.
For non-GCP prefer Cloud-Platform-managed keys, GCP rotates them once a day

### Cloud Identity (Global)

Identity as a Service (IDaaS) to provision and manage users and groups.
Centrally manage all users in Google Admin console

### Security Key Enforcement (Global)

USB or Bluetooth 2-step verification device that prevents phising

### Resource Manager (Global)

AWS organizations
Centrally manage & secure organization projects with custom folder hierarchy
- Organization: root
- Folders: per business needs

![Screenshot 2022-01-10 at 11 05 34](https://user-images.githubusercontent.com/39993930/148748325-63958b4a-dafe-4ff8-897f-7f8d785193d7.png)

Tied 1:1 to a Cloud Identity / G Suite domain without organzation specific identities people must own GCP projects.

### Cloud Identity-Aware Proxy (IAP) (Global)

AWS API Gateway
Guards running on GCP via identity verification not VPN access
Based on Cloud Load Balancer & IAM and only passes authed requests

### Cloud Audit Logging (Global)

AWS CloudTrail
Answers the questions: "Who did what, where and when" within GCP projects
Maintains activities for 400 days:
- Admin Activity (create GCE instance)
- Systems Events (auto-scaling)
- Access Transparency (GCP support)
30 days:
- Data Access

## Security Management

### Cloud Armor (Global)

AWS Shield & WAF (Web Application Firewall)
Edge-level protection from DDoS & others attacks on global HTTP(S) load balancers.
Monitor: Detailed request-level logs available in Stackdriver Logging
Manage IPs with CIDR-based allow / block lists (whitelist / blacklist)
Preview effect of changes before making them live

### Security Scanner (Global)

AWS Inspector
Free but limited Google App Engine app vulnerability scanner with "very low false positive rates"
Can identify:
- Cross-site-scripting (XSS)
- Flash injection
- Mixed content (HTTP in HTTPS)
- Oudated / insecure libraries

### Cloud DLP (Data Loss Prevention) API (Global)

AWS Macie

Finds and optionaly redacts sensitive info in unstructured data streams
50+ sensitive data detectors:
- names
- redit card  / social security / passport numbers

### ETD (Event Threat Dectection) (Global)

AWS GuardDuty
Automatically scans Stackdriver logs for suspicious activity
Detects threats:
- Malware
- cryptomining
- outgoing DDoS attacks
- brute-force SSH

Can export parsed logs to BigQuery for forensic analysis

### Cloud SCC (Security Command Center) (Global)

AWS Security Hub - Splunk ES - Sumo Logic
![Screenshot 2022-01-10 at 11 41 40](https://user-images.githubusercontent.com/39993930/148752868-f3e486d3-e05c-4f0f-a74a-fbed627d9f10.png)

Comprehensive security management and data risk platform for GCP


## Security Management

### Cloud KMS (Key Management Service) (Regional - Multi-Regional Global)

AWS KMS - HashiCorp Vault

Manage crypto keys
symmetric: AES
asymmetric: RSA, EC

Move secrets out of code and into the environmenet
Integrated with IAM & Cloud Audit Logging to authorize & track key usage
Rotate keys

### Cloud HSM (Hardware Security Moduke) (Regional - Multi-Regional Global)

Device hosts encryption keys and perform cryptographic operations
To meet compliance that mandates hardware environment
Fully integrated with Cloud KMS

## Operations & Management

### Stackdriver (Global)

AWS CloudWatch
Family of services for monitoring, logging & diagnosing apps on GCP / AWS / hybrid
1 Stackdriver account can track multiple:
- GCP projets
- AWS accounts
- Others resources

### Stackdriver Monitoring (Global)

AWS CloudWtach Metrics & Dashboards / Datadog / Collectd
Gives visivility into perf / uptime / overall health of cloud apps
Alerts via email / GCP mobille app / SMS / Slack ...
Cross-cloud

### Stackdriver Logging (Global)

AWS CloudWatch Logs / Splunk / Fluentd
Store, search, analyze monitor and alert on log data & events

### Stackdriver Error Reporting (Global)

Counts, analyzes, aggregates & tracks crashes in centralized interface


### Stackdriver Trace (Global)

AWS X-Ray
Tracks and displays call tree & timing across distributes systems

### Stackdriver Debugger (Global)

Grabs program state (callstack, varaibles, expressions) in live deploys

### Stackdriver Profiler (Global)

Continuous CPU and memory profiling to improve perf & reduce cost

### Stackdriver Profiler (Global)

AWS CloudFormation / Terraform

IaaS

### Cloud Billing API (Global)

AWS billing API
Programmatically manage billing for GCP

## Development & APIs

### Cloud Source Repositories (Global)

AWS CodeCommit / Github
Hosted private git repositories
Can set up automatic sync from Github
Natural integration with Stackdriver debugger

### Cloud Build (Global)

AWS CodeBuild, Travis, Jenkins
Continuously takes sources code and builds, tests, deploys it - CI / CD

### Container Registry (GCR) (Regional / Multi-Regional)

Private Docker image storage
Creates and manages a multi-regional GCS bucket, then translates GCR calls to GCS

### Cloud Endpoints (Global)

AWS API Gateway, Nginx

Handles authorization, monitoring, logging and API keys for APIs backed by GCP
Use JTWs

### Apigee (Global)

Full-featured, enterprise-scale API management platform for whole API lifecycle
Transforms calls between different protocols: SOAP / REST / XML
Authenticate: OAuth / SAML / LDAP
Authorize: RBAC

### Test Lab for Android (Global)

AWS Device Farm
Cloud infra for running test across variety of real Android devices
