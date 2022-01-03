# GCPAssociateCloudEngineer
Notes to pass the GCP associate cloud engineer certification

## Resources
1. [A Cloud Guru - GCP certified associate cloud engineer](https://learn.acloud.guru/course/gcp-certified-associate-cloud-engineer)
2. [GCP - AWS products mapping](https://cloud.google.com/free/docs/aws-azure-gcp-service-comparison)
3. [Every product in the Google Cloud family described in <=4 words](https://github.com/priyankavergadia/google-cloud-4-words)

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
