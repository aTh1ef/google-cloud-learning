# Google Cloud — Learning Notes (Fundamentals)

This document captures practical Google Cloud fundamentals learned through hands-on exploration: Projects, IAM, APIs, Cloud Storage, Cloud Shell, and Marketplace-based deployments.

---

## Index

- [Google Cloud at a glance](#google-cloud-at-a-glance)
  - [Console Navigation and Service Discovery](#console-navigation-and-service-discovery)
- [Projects (resource boundaries)](#projects-resource-boundaries)
- [IAM (Identity & Access Management)](#iam-identity--access-management)
- [APIs & Services](#apis--services)
- [Cloud Storage basics](#cloud-storage-basics)
- [Cloud Shell essentials](#cloud-shell-essentials)
  - [Console vs. Cloud Shell Mindset](#console-vs-cloud-shell-mindset)
  - [Cloud Shell UI Controls](#cloud-shell-ui-controls)
- [Marketplace deployments (quick infrastructure)](#marketplace-deployments-quick-infrastructure)
  - [Deployment Verification](#deployment-verification)
- [Quick command reference](#quick-command-reference)

---

## Google Cloud at a glance

Google Cloud (GCP) is a suite of cloud services hosted on Google’s infrastructure, spanning compute, storage, networking, analytics, and machine learning services.  
The Cloud Console is the web UI used to access and manage Google Cloud services and projects from a single place.

### Console Navigation and Service Discovery

The **Navigation menu** (often referred to as the hamburger icon) in the top-left corner of the Console is the primary way to discover and access all Google Cloud products and services. You can explore service categories by clicking the menu or by using the search field to quickly locate a specific product.

---

## Projects (resource boundaries)

A **Google Cloud project** is an organizing boundary for resources (VMs, storage, networking) and also holds settings and permissions that control access.  
Projects have identifiers commonly used across services: **Project name**, **Project number**, and **Project ID** (the Project ID is a unique identifier used to link resources and APIs to the project).

**Practical implications**
- Billing, APIs, IAM policies, and quotas are commonly applied at the project level.
- When switching work contexts, always verify the current project in the Console project selector before creating resources.

---

## IAM (Identity & Access Management)

IAM controls *who* (principal) can do *what* (permissions) on *which resources* by assigning roles.  
Basic roles described include Viewer, Editor, and Owner—each progressively increasing permissions and administrative control.

### Basic roles (quick reference)

| Role | Role ID | What it allows |
|------|--------|----------------|
| Viewer | `roles/viewer` | Read-only actions that don’t affect state (view resources/data but not modify). |
| Editor | `roles/editor` | Viewer permissions + modify state (change existing resources). |
| Owner | `roles/owner` | Editor permissions + manage roles/permissions and set up billing. |

### Common IAM actions (what was practiced conceptually)

- Inspecting principals and their roles in **IAM & Admin → IAM**.
- Granting a role by adding a principal and assigning a role (example: granting Viewer access).

---

## APIs & Services

Google Cloud provides many APIs that integrate with projects and applications, and projects often require explicitly enabling APIs before they can be used.  
The **APIs & Services → Library** in the Console is used to browse categories, search APIs, and enable them.  
Cloud APIs provide metrics/visibility such as usage, traffic levels, error rates, and latencies to help troubleshoot applications using Google services.

### Example: Dialogflow API (what this illustrates)

The Dialogflow API can be enabled from the API Library and is used to build conversational applications without needing to manage underlying ML/NLP schema directly.

---

## Cloud Storage basics

A Cloud Storage **bucket** is a globally unique storage container name in Google’s object storage service (bucket names must be globally unique).  
Buckets can be created through the Console (GUI) and also through Cloud Shell using the `gcloud storage` CLI.

### Practical operations covered

- Create a bucket in the Console (Cloud Storage → Buckets → Create).
- Create a bucket using Cloud Shell:
  - `gcloud storage buckets create gs://[BUCKET_NAME]`
- Upload a local file to Cloud Shell and copy it into a bucket:
  - **File Upload Workflow:**
    1. Use the **Upload** button in the Cloud Shell menu to upload a local file (e.g., `my_file.txt`) to the home directory.
    2. Verify the file is present using `ls`.
    3. Copy the file into a bucket:
       - `gcloud storage cp [MY_FILE] gs://[BUCKET_NAME]`

---

## Cloud Shell essentials

Cloud Shell is a browser-accessible command-line environment for managing Google Cloud resources without installing the Cloud SDK locally.  
Cloud Shell includes a temporary VM, built-in authorization, and persistent storage in the home directory.

### Console vs. Cloud Shell Mindset

The Google Cloud interface consists of two parts: the Cloud Console and Cloud Shell. Each is suited for different tasks:

| Interface | Primary Use Case | Key Features |
|---|---|---|
| **Cloud Console** | Fast, interactive tasks and configuration. | Presents options, performs behind-the-scenes validation, and is quick to use. |
| **Cloud Shell** | Detailed control, scripting, and automation. | Complete range of options, path to automation through scripting, and detailed control over resources. |

### Cloud Shell UI Controls

The Cloud Shell window provides several controls for managing the terminal session:
- **Minimize/Maximize:** Adjusts the size of the terminal window within the browser.
- **Open in new window:** Launches the terminal into a separate, full-screen browser tab.
- **Close terminal:** Ends the current session and recycles the temporary VM.

### What Cloud Shell provides (key capabilities)

- Temporary Compute Engine VM + command-line access via browser.
- 5 GB of persistent disk storage in the `$HOME` directory.
- Pre-installed tools such as:
  - `gcloud` (for many Google Cloud services)
  - `gcloud storage` (Cloud Storage operations)
  - `kubectl` (GKE/Kubernetes)
  - `bq` (BigQuery)
- Web preview functionality (useful when running a web server inside Cloud Shell).

### Session behavior + persistence pattern

After inactivity, the Cloud Shell VM can be recycled; the home directory persists, but system-level configuration and environment variables do not automatically persist between sessions.  
A persistence strategy demonstrated is storing environment variables in a file (example: `~/infraclass/config`) and sourcing it from `.profile` so variables reload when a new shell starts.

**Example pattern**
```bash
mkdir -p infraclass
touch infraclass/config

# append values
echo INFRACLASS_REGION=[YOUR_REGION] >> ~/infraclass/config
echo INFRACLASS_PROJECT_ID=[YOUR_PROJECT_ID] >> ~/infraclass/config

# load them into current shell
source infraclass/config

# make it auto-load for new sessions
nano .profile
# add:
# source infraclass/config
```

---

## Marketplace deployments (quick infrastructure)

Google Cloud Marketplace can deploy complete solution stacks using pre-defined deployment templates to provision infrastructure quickly.  
An example deployment covered is a **LAMP stack** deployed onto a Compute Engine VM.

### Deployment Verification

After the deployment is complete, the status summary provides a **Site Url** link. Clicking this link and navigating to the site confirms that the web server (Apache HTTP Server) is running successfully, completing the deployment as a functional learning note.

### LAMP stack components (as packaged)

| Component | Role |
|----------|------|
| Linux | Operating system |
| Apache HTTP Server | Web server |
| MySQL | Relational database |
| PHP | Web application framework/runtime |
| phpMyAdmin | PHP administration tool |

---

## Quick command reference

### List regions
```bash
gcloud compute regions list
```

### Create a bucket
```bash
gcloud storage buckets create gs://[BUCKET_NAME]
```

### Copy a file to a bucket
```bash
gcloud storage cp [MY_FILE] gs://[BUCKET_NAME]
```
