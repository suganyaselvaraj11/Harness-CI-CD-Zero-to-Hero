# Terraform Project — Create AWS Infrastructure (OIDC, No Access Keys!)

## What This Does

```
Run pipeline → Choose "apply" → Creates VPC + EC2 in AWS
Run pipeline → Choose "destroy" → Deletes everything, bill = $0
```

No Delegate needed. No AWS Access Keys stored. Uses OIDC!

---

## How It Works (The Flow)

```
GitHub (stores your Terraform code)
    ↓
Harness (clones code using GitHub connector)
    ↓
Terraform (reads main.tf from cloned code)
    ↓
AWS (creates VPC + EC2 using OIDC authentication)

Two connectors used:
  account.Github        → Clones your code from GitHub
  account.aws_connector → Authenticates to AWS via OIDC (no keys!)
```

---

## Why OIDC Instead of Access Keys?

```
Access Keys (BAD):
  - Stored as secrets in Harness
  - If someone hacks your Harness account → they have your AWS keys
  - Keys don't expire unless you rotate them manually

OIDC (GOOD):
  - No keys stored anywhere
  - Harness proves its identity to AWS directly
  - Temporary credentials (expire in 1 hour automatically)
  - Even if hacked, credentials are already expired
```

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

1. Go to AWS Console → S3 → Create bucket
2. Fill in:
   - Bucket name: `harness-terraform-project`
   - Region: `us-east-1`
   - Versioning: Enable ✅
3. Click Create bucket

---

### Step 2: Create OIDC Provider in AWS

1. Go to AWS Console → IAM → Identity providers
2. Click Add provider
3. Fill in:
   - Provider type: OpenID Connect
   - Provider URL: `https://app.harness.io/ng/api/oidc/account/YOUR_HARNESS_ACCOUNT_ID`
   - Click "Get thumbprint"
   - Audience: `sts.amazonaws.com`
4. Click Add provider

---

### Step 3: Create IAM Role for Harness

1. Go to IAM → Roles → Create role
2. Trusted entity: Web identity
3. Identity provider: select `app.harness.io/ng/api/oidc/account/...`
4. Audience: `sts.amazonaws.com`
5. Click Next
6. Permissions: AdministratorAccess
7. Role name: `harness-oidc-role`
8. Create role
9. Copy ARN: `arn:aws:iam::YOUR_ACCOUNT_ID:role/harness-oidc-role`

---

### Step 4: Create AWS Connector in Harness (OIDC)

1. Account Settings → Connectors → + New Connector → AWS
2. Name: `aws-connector`
3. Credentials: Select **Use OIDC**
4. IAM Role: `arn:aws:iam::YOUR_ACCOUNT_ID:role/harness-oidc-role`
5. Test Region: `us-east-1`
6. Backoff: keep defaults
7. Connectivity: Connect through Harness Platform
8. Test → ✅

---

### Step 5: Add Variables in Harness

Project Settings → Variables:

| Variable | Value |
|----------|-------|
| `s3_bucket_name` | `harness-tf-state-YOUR-ACCOUNT-ID` |
| `aws_region` | `us-east-1` |

No secrets needed! OIDC handles AWS authentication.

---

### Step 6: Import Pipeline

1. Pipelines → Import from Git
2. Connector: Github
3. Repo: Harness-CI-CD-Zero-to-Hero
4. Branch: master
5. YAML Path: `Episode-03/terraform-project/.harness/pipeline-terraform.yaml`
6. Click Import

---

## How to Run

### CREATE infrastructure:
1. Run pipeline → tf_action: `apply` → Wait 2 min → Done ✅

### DESTROY infrastructure:
1. Run pipeline → tf_action: `destroy` → Wait 1 min → Bill = $0 ✅

---

## Where is the State File?

```
S3 bucket: YOUR-BUCKET-NAME
Path: harness-demo/terraform.tfstate

Terraform uses this to know what it created.
Destroy reads this to know what to delete.
```

---

## Cost

```
Running: $0 (t2.micro = free tier)
Destroyed: $0
```

---

## Summary — No Secrets Needed!

| What | Where |
|------|-------|
| S3 bucket | AWS Console (manual, once) |
| OIDC Provider | AWS IAM (manual, once) |
| IAM Role | AWS IAM (manual, once) |
| AWS Connector (OIDC) | Harness Connectors |
| Variable `s3_bucket_name` | Harness Variables |
| Variable `aws_region` | Harness Variables |
| ~~aws-access-key~~ | ~~NOT NEEDED~~ ❌ |
| ~~aws-secret-key~~ | ~~NOT NEEDED~~ ❌ |
