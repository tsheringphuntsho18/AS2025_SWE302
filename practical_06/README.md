# Practical_06 Report: Infrastructure as Code with Terraform and LocalStack

## Overview
This practical demonstrates how to define, deploy, and secure cloud infrastructure using Infrastructure as Code (IaC). The exercise covers provisioning AWS S3 buckets locally via LocalStack using Terraform, deploying a Next.js static website, and scanning infrastructure code for security vulnerabilities with Trivy.

## Learning Outcomes
- Use Terraform to define and provision infrastructure on LocalStack AWS.
- Deploy a Next.js static website to AWS S3 using IaC.
- Use Trivy to scan Infrastructure as Code for security vulnerabilities.

## Technologies Used
- **Terraform**: Infrastructure as Code tool for defining cloud resources.
- **LocalStack**: Local AWS cloud emulator.
- **AWS S3**: Object storage and static website hosting.
- **Next.js**: React framework for static site generation.
- **Trivy**: Security scanner for IaC and containers.

## Prerequisites
- Docker & Docker Compose
- Terraform (>= 1.0)
- terraform-local (`tflocal`)
- Node.js (>= 18)
- AWS CLI & `awslocal`
- Trivy
- Visual Studio Code (recommended)

Verify installation with:
```sh
docker --version
docker-compose --version
terraform --version
tflocal --version
node --version
npm --version
aws --version
awslocal --version
trivy --version
```

## Project Structure
```
practical_06/
├── docker-compose.yml
├── Makefile
├── trivy.yaml
├── scripts/
│   ├── setup.sh
│   ├── deploy.sh
│   ├── status.sh
│   ├── cleanup.sh
│   ├── scan.sh
│   └── compare-security.sh
├── nextjs-app/
│   ├── app/
│   ├── next.config.js
│   └── package.json
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── s3.tf
│   ├── iam.tf
│   └── outputs.tf
└── terraform-insecure/
    ├── s3-insecure.tf
    ├── iam-insecure.tf
    └── README.md
```

## Quick Start

### Automated Deployment
```sh
make init         # Install dependencies
make deploy       # Deploy infrastructure and app
make status       # Check deployment status
curl $(cd terraform && terraform output -raw deployment_website_endpoint)  # View website
```

### Manual Steps
```sh
./scripts/setup.sh
cd nextjs-app && npm ci && npm run build && cd ..
cd terraform && tflocal init && tflocal apply && cd ..
awslocal s3 sync nextjs-app/out/ s3://$(cd terraform && terraform output -raw deployment_bucket_name)/ --delete
./scripts/status.sh
```

## Terraform Configuration Highlights
- **Provider**: Configured for LocalStack endpoints and test credentials.
- **S3 Deployment Bucket**: Website hosting, public read, server-side encryption (AES256).
- **S3 Logs Bucket**: Stores access logs, encrypted.
- **Bucket Policies**: Explicit public read for website, logging enabled.

## Security Scanning with Trivy

### Scan Secure Configuration
```sh
./scripts/scan.sh terraform
# or
make scan
```

### Scan Insecure Configuration
```sh
./scripts/scan.sh insecure
# or
make scan-insecure
```

### Compare Results
```sh
./scripts/compare-security.sh
# or
make compare-security
```

## Exercises

### 1. Modify the Next.js Application
- Edit `nextjs-app/app/page.tsx`
- Redeploy: `make dev`
- Verify: `curl $(cd terraform && terraform output -raw deployment_website_endpoint)`

### 2. Fix a Security Issue
- Choose a finding from Trivy scan in `terraform-insecure/`
- Apply the recommended fix
- Re-scan: `./scripts/scan.sh insecure`

### 3. Add a New S3 Bucket
- Add a `backups` bucket in `terraform/s3.tf`
- Add encryption
- `tflocal plan` and `tflocal apply`
- Verify: `awslocal s3 ls | grep backups`

### 4. Implement Versioning
- Add versioning to deployment bucket in `terraform/s3.tf`
- Apply and verify:  
  `awslocal s3api get-bucket-versioning --bucket practical6-deployment-dev`

### 5. Monitor Website Access
- Access website, then check logs in logs bucket:
  `awslocal s3 ls s3://practical6-logs-dev/deployment-logs/`


## Troubleshooting
- **Website not accessible**: Check S3 files, bucket policy, website config, and index.html.
- **Terraform apply fails**: Ensure LocalStack is running, check logs, destroy and re-apply.
- **Build fails**: Clean and reinstall dependencies in `nextjs-app`.

## Clean Up
```sh
make clean
# or
./scripts/cleanup.sh
```

## Reflection
**Why is it important to scan IaC for security issues?**  
Scanning IaC ensures that infrastructure is provisioned securely, preventing misconfigurations that could lead to data breaches or service disruptions.

**How does LocalStack help in the development workflow?**  
LocalStack enables rapid, cost-effective local testing of AWS infrastructure, allowing developers to validate changes before deploying to real cloud environments.

## Key Takeaways
- IaC makes infrastructure reproducible and version-controlled.
- Security should be integrated from the start.
- Automated scanning catches issues early.
- LocalStack enables local development without cloud costs.
- Terraform provides a consistent way to manage cloud resources.


