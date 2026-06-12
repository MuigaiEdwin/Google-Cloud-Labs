# Lab 03 — Getting Started with Cloud Storage and Cloud SQL

## Overview
Deployed a web server on Compute Engine, created a Cloud Storage bucket
to host a public image, provisioned a Cloud SQL (MySQL) instance, and
connected the web application to the database — all wired together into
a working blog page.

## What I Did

### Task 1 — Deployed a Web Server VM
- Created VM instance `bloghost` on Compute Engine (e2-standard-2)
- Used a startup script to auto-install Apache, PHP, and php-mysql
- Enabled HTTP traffic via firewall rule
*(startup scripts in GCP = EC2 User Data in AWS)*

### Task 2 — Created a Cloud Storage Bucket
- Used Cloud Shell and `gcloud storage` CLI to create a bucket
  named after the Project ID — globally unique
- Set bucket location to multi-region (US/EU/ASIA)
- Uploaded a public image using `gcloud storage cp`
- Made the object publicly readable via ACL:

```bash
gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
```

### Task 3 — Created a Cloud SQL Instance
- Provisioned a MySQL instance `blog-db` in Cloud SQL (Enterprise, Sandbox)
- Created database user `blogdbuser`
- Authorized the external IP of `bloghost` VM as a trusted network
  connection (`<external-ip>/32`)

### Task 4 — Connected Web App to Cloud SQL
- SSH'd into `bloghost` and edited `/var/www/html/index.php`
- Injected Cloud SQL public IP and database credentials into PHP config
- Restarted Apache — confirmed `Connected successfully` in browser

### Task 5 — Served Image from Cloud Storage
- Retrieved the public URL of the image in the Cloud Storage bucket
- Embedded it as an `<img src='...'>` tag in `index.php`
- Final page loaded with banner image served directly from Cloud Storage

## Key Concepts Learned
- **Cloud Storage** — object storage for any file type, globally
  accessible *(equivalent to AWS S3)*
- **Cloud SQL** — fully managed relational database (MySQL/PostgreSQL)
  *(equivalent to AWS RDS)*
- **Startup scripts** — automate VM configuration on boot
  *(equivalent to EC2 User Data in AWS)*
- **ACL (Access Control List)** — controls public/private access
  to individual objects in Cloud Storage
- **Authorized networks** — whitelist specific IPs to access
  Cloud SQL *(similar to RDS Security Group inbound rules)*
- **`/32` CIDR** — means a single exact IP address

## GCP → AWS Equivalent

| GCP Service | AWS Equivalent |
|-------------|----------------|
| Cloud Storage | S3 |
| Cloud SQL (MySQL) | RDS (MySQL) |
| Compute Engine | EC2 |
| Startup Script | EC2 User Data |
| Cloud Shell | AWS CloudShell |
| Authorized Networks | RDS Security Group Rules |
| Object ACL (allUsers:R) | S3 Bucket Policy (Public Read) |

## Commands Used

```bash
# Create bucket at multi-region location
gcloud storage buckets create -l $LOCATION gs://$DEVSHELL_PROJECT_ID

# Copy image from public GCS bucket
gcloud storage cp gs://cloud-training/gcpfci/my-excellent-blog.png .

# Upload to own bucket
gcloud storage cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

# Make object publicly readable
gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png
```

## Result
✅ Web server connected to Cloud SQL database and serving an image
from Cloud Storage — full 3-tier architecture running on GCP.
