# 01 - Set up GitHub secrets

The workflows authenticate to AWS with two repository secrets. No key pair secret is needed; CloudFormation creates the key pair.

## 1. Get the keys from the lab page

1. On the lab page, click **Details**, then click **Show** next to **Credentials**.
2. Copy the **AccessKey** value (starts with `AKIA`, no `/`).
3. Copy the **SecretKey** value.

> Don't swap the two values or add spaces or newlines when pasting. Either mistake makes the workflow fail with a signature error.

## 2. Add them to GitHub

Go to your repo → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**.

| Secret name             | Value                      |
| ----------------------- | -------------------------- |
| `AWS_ACCESS_KEY_ID`     | Your AWS Access Key ID     |
| `AWS_SECRET_ACCESS_KEY` | Your AWS Secret Access Key |

## 3. Check the region

The workflows default to `us-east-1`. If your lab uses another region, change `AWS_REGION` in both files under `.github/workflows/`.

## Troubleshooting

- **SignatureDoesNotMatch / invalid token**: the keys were swapped, have stray whitespace, or the lab session expired. Re-copy them and update the secrets.
- If your lab provides a **session token** as well, add an `AWS_SESSION_TOKEN` secret and pass `aws-session-token: ${{ secrets.AWS_SESSION_TOKEN }}` in the "Configure AWS credentials" step of both workflows.

Next: [02 - Deploy and connect](02-deploy-and-connect.md)
