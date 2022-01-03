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
