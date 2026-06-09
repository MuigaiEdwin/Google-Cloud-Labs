# Lab 01 — Deploy a LAMP Stack with Cloud Marketplace

## Overview
Used Google Cloud Marketplace to deploy a LAMP stack on a Compute Engine 
VM instance using the **Google Click to Deploy** solution — a complete 
web development environment for Linux launched in one click.

## Stack Components

| Component | Role |
|-----------|------|
| Linux | Operating system |
| Apache HTTP Server | Web server |
| MySQL | Relational database |
| PHP | Web application framework |
| phpMyAdmin | PHP administration tool |

## What I Did
- Navigated to Cloud Marketplace and searched for LAMP stack
- Deployed using Google Click to Deploy to a Compute Engine VM (`lamp-1-vm`)
- Verified the deployment by accessing the site URL
- Confirmed Apache HTTP Server was running via the browser

## Key Concepts Learned
- **Cloud Marketplace** — pre-configured solutions deployable instantly
  without manual setup *(similar to AWS Marketplace)*
- **Compute Engine** — Google Cloud's virtual machine service
  *(equivalent to EC2 in AWS)*
- **LAMP stack** — a common web server environment for hosting
  Linux-based web applications
- Deployments get a public site URL and an external IP address

## GCP → AWS Equivalent
| GCP Service | AWS Equivalent |
|-------------|----------------|
| Cloud Marketplace | AWS Marketplace |
| Compute Engine | EC2 |

## Result
✅ Apache HTTP Server confirmed running on a live Compute Engine instance.
