# AWS EC2 with CloudFormation and GitHub Actions

Deploy an EC2 instance to an AWS lab account using a CloudFormation template, driven by GitHub Actions.

## Repository layout

```
.
├── .github/workflows/
│   └── deploy.yml        # validate + deploy the stack (push to main or manual)
├── templates/
│   └── ec2-instance.yaml # CloudFormation: key pair, security group, EC2 instance
└── docs/
    ├── 01-setup-github-secrets.md
    └── 02-deploy-and-connect.md
```

## What gets created

- An EC2 key pair (created by CloudFormation; private key stored in SSM Parameter Store)
- A security group allowing SSH (22) and HTTP (80)
- One Amazon Linux 2023 EC2 instance (default `t2.micro`)

## Quick start

1. Follow [docs/01-setup-github-secrets.md](docs/01-setup-github-secrets.md) to add your lab credentials as GitHub secrets.
2. Follow [docs/02-deploy-and-connect.md](docs/02-deploy-and-connect.md) to deploy and SSH in.

## Configuration

Defaults live in the `env` block of the workflow files (`AWS_REGION`, `STACK_NAME`). Template parameters (`InstanceType`, `SSHLocation`) are in [templates/ec2-instance.yaml](templates/ec2-instance.yaml).

## Notes

- Lab credentials expire when the lab session ends. Update the secrets each time you start a new lab.
- Never commit credentials or `.pem` files; `.gitignore` excludes the latter.
