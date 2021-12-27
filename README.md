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
