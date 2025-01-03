# Google Cloud HTTP Load Balancer Implementation
A technical implementation of an HTTP Load Balancer with managed instance groups in Google Cloud Platform.

## Project Overview
Implementation of a fault-tolerant HTTP load balancing setup using nginx web servers in GCP, consisting of:
- A jumphost instance for project maintenance
- An HTTP load balancer
- A managed instance group with nginx web servers

## Architecture Components

### 1. Jumphost Instance
- Purpose: Project maintenance
- Machine type: e2-micro
- OS: Debian Linux
- Zone: Default project zone

### 2. HTTP Load Balancer Setup
Components created in the following order:

1. **Instance Template**
   - Machine type: e2-medium
   - Location: Global
   - Custom startup script for nginx installation

2. **Managed Instance Group**
   - Based on instance template
   - Size: 2 instances
   - Health check enabled

3. **Network Configuration**
   - Firewall rule: Allow HTTP (80/tcp)
   - Health check configuration
   - Backend service with named port (http:80)
   - URL map for request routing
   - HTTP proxy target
   - Frontend forwarding rule

## Implementation Details

### Startup Script
```bash
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
```

### Key Considerations
- Resource sizing follows cost-effective guidelines
- All resources created in default region/zone unless specified
- Global instance template requirement

## Project Standards
- Resource naming convention: team-resource
- Default zone and region usage
- Cost-effective resource allocation
- Global template location requirement
