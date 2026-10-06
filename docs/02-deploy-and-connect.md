# 02 - Deploy and connect

Prerequisite: [01 - Set up GitHub secrets](01-setup-github-secrets.md).

## Deploy

The **Deploy EC2 stack** workflow runs automatically on pushes to `main` that change `templates/` or the workflow itself. To run it manually:

1. Go to the **Actions** tab → **Deploy EC2 stack** → **Run workflow**.
2. Optionally pick an instance type.

The workflow validates the template, deploys the stack `ec2-lab-stack`, and prints the outputs (instance ID, public IP, DNS, key pair ID) in the run summary.

## Connect over SSH

The private key is stored in SSM Parameter Store. Using the AWS CLI configured with your lab credentials:

```bash
KEY_ID=<KeyPairId from the workflow outputs>
aws ssm get-parameter --name /ec2/keypair/$KEY_ID --with-decryption \
  --query Parameter.Value --output text --region us-east-1 > ec2-key.pem
chmod 400 ec2-key.pem
ssh -i ec2-key.pem ec2-user@<PublicIp>
```

## Update the stack

Edit [templates/ec2-instance.yaml](../templates/ec2-instance.yaml), commit, and push to `main`. CloudFormation applies the change set.

To restrict SSH to your IP, change the `SSHLocation` default to `<your-ip>/32`.

## Troubleshooting

- **Stack in `ROLLBACK_COMPLETE`**: delete it from the CloudFormation console, then re-run deploy.
- **Template errors**: check the "Validate template" step output.
