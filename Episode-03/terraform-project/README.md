# Terraform Project — Create AWS Infrastructure from Harness Pipeline

## What This Does

```
Run pipeline → Choose "apply" → Creates VPC + EC2 in AWS
Run pipeline → Choose "destroy" → Deletes everything, bill = $0
```

No Delegate needed. Uses Harness Cloud.

---

## What Gets Created in AWS

```
VPC (10.0.0.0/16)
├── Internet Gateway
├── Public Subnet (10.0.1.0/24)
├── Route Table (internet access)
├── Security Group (all ports open)
└── EC2 Instance (t2.micro, Amazon Linux 2023)
```

---

## Setup — Do These ONCE Before Running

---

### Step 1: Create S3 Bucket (Terraform State)

This is where Terraform saves "what it created" so it can destroy later.

1. Go to AWS Console: https://console.aws.amazon.com
2. Search for "S3" → Click S3
3. Click "Create bucket"
4. Fill in:
   - Bucket name: `harness-tf-state-YOUR-ACCOUNT-ID` (must be unique worldwide)
   - Region: `us-east-1`
   - Bucket Versioning: Enable ✅
   - Everything else: leave default
5. Click "Create bucket"

Done. Remember this bucket name — you'll add it in Harness.

---

### Step 2: Get AWS Access Key + Secret Key

1. Go to AWS Console → IAM → Users
2. Click on your user (or create new user)
3. Click "Security credentials" tab
4. Click "Create access key"
5. Select "Command Line Interface (CLI)"
6. Click "Create access key"
7. You see:
   - Access key ID: `AKIA...` (copy this)
   - Secret access key: `wJalr...` (copy this)
8. Save both somewhere safe

---

### Step 3: Add AWS Secrets in Harness

1. Go to Harness → Your Project → Project Settings → Secrets
2. Click "+ New Secret" → Text

**Secret 1:**
   - Name: `aws-access-key`
   - Value: paste your Access Key ID (`AKIA...`)
   - Click Save

3. Click "+ New Secret" → Text again

**Secret 2:**
   - Name: `aws-secret-key`
   - Value: paste your Secret Access Key (`wJalr...`)
   - Click Save

---

### Step 4: Add Variables in Harness

1. Go to Project Settings → Variables
2. Click "+ New Variable"

**Variable 1:**
   - Name: `s3_bucket_name`
   - Type: String
   - Value: `harness-tf-state-YOUR-ACCOUNT-ID` (the bucket you created in Step 1)
   - Click Save

3. Click "+ New Variable" again

**Variable 2:**
   - Name: `aws_region`
   - Type: String
   - Value: `us-east-1`
   - Click Save

---

### Step 5: Import Pipeline in Harness

1. Go to Pipelines → Import from Git
2. Connector: Github
3. Repo: Harness-CI-CD-Zero-to-Hero
4. Branch: master
5. YAML Path: `Episode-03/terraform-project/.harness/pipeline-terraform.yaml`
6. Click Import

---

## How to Run

### To CREATE infrastructure:

1. Click "Run" on the pipeline
2. Branch: master
3. When it asks for `tf_action`: type `apply`
4. Click "Run Pipeline"
5. Wait 2-3 minutes
6. Output shows: VPC ID + EC2 Public IP ✅

### To DESTROY infrastructure:

1. Click "Run" on the same pipeline
2. Branch: master
3. When it asks for `tf_action`: type `destroy`
4. Click "Run Pipeline"
5. Wait 1-2 minutes
6. Output shows: "DESTROYED! Bill = $0" ✅

---

## Expected Output

### Apply:
```
=== CREATED!
vpc_id = "vpc-0abc123..."
ec2_public_ip = "54.123.45.67"
ec2_instance_id = "i-0abc123..."
=========================================
```

### Destroy:
```
=== DESTROYED! All resources deleted.
AWS bill for these resources: $0
=========================================
```

---

## Where is the State File?

```
Location: S3 bucket
Bucket: YOUR-BUCKET-NAME
File: harness-demo/terraform.tfstate

Why S3?
  - Harness Cloud has no permanent disk
  - Each pipeline run is fresh (nothing saved locally)
  - S3 keeps the state between runs
  - Without S3, Terraform forgets what it created!
```

---

## Cost

```
When RUNNING (after apply):
  EC2 t2.micro: FREE (750 hours/month free tier)
  VPC: FREE
  Total: $0 (if within free tier)

After DESTROY:
  $0 (everything deleted)
```

---

## Project Structure

```
Episode-03/terraform-project/
├── README.md                          ← This file
├── main.tf                            ← VPC + EC2 Terraform code
└── .harness/
    └── pipeline-terraform.yaml        ← Harness pipeline (3 steps)
```

---

## Summary

| What | Where | How |
|------|-------|-----|
| S3 bucket | AWS Console → S3 | Create manually once |
| Secret `aws-access-key` | Harness → Project Settings → Secrets | Your AWS Access Key ID |
| Secret `aws-secret-key` | Harness → Project Settings → Secrets | Your AWS Secret Access Key |
| Variable `s3_bucket_name` | Harness → Project Settings → Variables | Your S3 bucket name |
| Variable `aws_region` | Harness → Project Settings → Variables | `us-east-1` |
| Pipeline | Harness → Import from Git | YAML Path in repo |

---

## Troubleshooting

| Error | Fix |
|-------|-----|
| "No valid credential sources" | Check secrets `aws-access-key` and `aws-secret-key` are correct |
| "S3 bucket does not exist" | Create the S3 bucket in AWS first (Step 1) |
| "Access Denied" | Make sure your AWS user has AdministratorAccess or EC2+VPC permissions |
| "Backend initialization required" | The S3 bucket name in variable must match exactly |
| Destroy says "no state" | You must apply first before you can destroy |
