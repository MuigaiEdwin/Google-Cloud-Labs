# Lab 02 >> Getting Started with VPC Networking and Compute Engine

## Overview
Explored Google Cloud VPC networking by working with the default network,
creating a custom VPC network, deploying VM instances, and testing
connectivity through firewall rules.

## What I Did

### Task 1 >> Explored the Default Network
- Viewed default subnets across all GCP regions
- Explored default routes and firewall rules
- Deleted all default firewall rules then deleted the default VPC network
- Confirmed that VM creation fails without a VPC network — no network,
  no routes, no firewall rules, no instances

### Task 2 >> Created a Custom VPC Network
- Created `mynetwork` as an auto mode VPC — subnets provisioned
  automatically per region *(similar to a default VPC in AWS)*
- Enabled all standard firewall rules: allow-icmp, allow-ssh,
  allow-rdp, allow-custom
- Deployed two VM instances:
  - `mynet-us-vm` — Region 1
  - `mynet-r2-vm` — Region 2
- Both VMs assigned ephemeral external IPs *(like elastic IPs in AWS
  but released on stop)*

### Task 3 >> Tested Connectivity and Firewall Behaviour
- SSH'd into `mynet-us-vm` and pinged `mynet-r2-vm` on both
  internal and external IP — both worked
- Deleted `allow-icmp` → external ping failed, internal still worked
- Deleted `allow-custom` → internal ping failed completely
- Deleted `allow-ssh` → SSH connection refused entirely

## Key Concepts Learned
- **VPC** >> isolated private network in GCP *(same concept as AWS VPC)*
- **Auto mode VPC** >> subnets created automatically in every region
- **Firewall rules** >> stateless, applied at network level in GCP
  *(similar to Security Groups in AWS but more granular)*
- **Ephemeral vs Static external IPs** >> ephemeral IPs released on
  VM stop, static IPs reserved until explicitly released
  *(equivalent to Elastic IPs in AWS)*
- **Routes** >> automatically managed per subnet, control traffic flow
  inside and outside the VPC

## GCP → AWS Equivalent

| GCP Service | AWS Equivalent |
|-------------|----------------|
| VPC Network | VPC |
| Subnets (auto mode) | Default Subnets |
| Firewall Rules | Security Groups / NACLs |
| Compute Engine VM | EC2 Instance |
| Ephemeral External IP | Auto-assigned Public IP |
| Static External IP | Elastic IP (EIP) |
| Routes | Route Tables |

## Result
✅ Custom VPC network created, VMs deployed across two regions,
firewall rule impact on connectivity confirmed.
