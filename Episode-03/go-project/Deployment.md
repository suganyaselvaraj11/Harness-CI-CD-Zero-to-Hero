# Episode 3: Docker Delegate on AWS EC2 + Go CI Pipeline with Auto Rollback

## 🎯 Goal
Install a Docker Delegate on AWS EC2 and run a Go CI pipeline with automatic rollback on failure.

---

## What We Build

```
AWS EC2 Instance (Ubuntu)
    ├── Docker installed
    ├── Harness Docker Delegate (connected to Harness)
    ├── Harness Docker Runner (runs pipeline steps)
    └── /tmp/harness/last-successful-tag.txt (rollback data)

Pipeline (7 steps + automatic rollback):
    Save Rollback Data
        → Test Go Code
            → Build Binary
                → Verify App
                    → Push to Docker Hub
                        → Update Rollback Data
                            → Success!

    If ANY step fails:
        → Automatic Rollback triggers
        → Delete failed image tag
        → Keep previous stable version
```

---

## How Rollback Works

```
Build #1: Tests pass → Push tag:1 → Save "1" as last successful → Docker Hub has: tag 1 + latest
Build #2: Tests pass → Push tag:2 → Save "2" as last successful → Docker Hub has: tag 1, 2 + latest
Build #3: Tests FAIL → Rollback! → Delete tag:3 → Keep tag:2 as latest → Docker Hub has: tag 1, 2 + latest
Build #4: Tests pass → Push tag:4 → Save "4" as last successful → Docker Hub has: tag 1, 2, 4 + latest
```

**Where is rollback data stored?**
```
EC2 location: /tmp/harness/last-successful-tag.txt
Contains: just one number (the last successful build number)

To check it on EC2:
ssh -i harness-key.pem ubuntu@YOUR-EC2-IP
cat /tmp/harness/last-successful-tag.txt
```

---

## What You Need Before Starting

1. AWS Account (free tier works)
2. Harness account
3. GitHub connector already created (account.Github)
4. Docker Hub connector already created (dockerhub at project level)
5. Secret `docker-hub-password` created
6. Variable `docker_username` created

---

## Step 1: Create AWS EC2 Instance

1. Login to AWS Console: https://console.aws.amazon.com
2. Go to EC2 → Launch Instance
3. Fill in:
   - Name: `harness-delegate`
   - OS: Ubuntu 22.04 LTS (free tier eligible)
   - Instance type: `t2.medium` (2 CPU, 4 GB RAM — minimum for delegate)
   - Key pair: Create new → name: `harness-key` → Download .pem file
   - Security Group: Allow SSH (port 22) from your IP
4. Click Launch Instance
5. Wait until status shows: Running ✅

---

## Step 2: Connect to EC2 via SSH

```bash
chmod 400 harness-key.pem
ssh -i harness-key.pem ubuntu@YOUR-EC2-PUBLIC-IP
```

You are now inside the EC2 machine.

---

## Step 3: Install Docker on EC2

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
exit
```

SSH back in:
```bash
ssh -i harness-key.pem ubuntu@YOUR-EC2-PUBLIC-IP
```

Verify:
```bash
docker --version
docker ps
```

---

## Step 4: Install Harness Docker Delegate on EC2

1. Go to Harness → Project Settings → Delegates → + New Delegate → Docker
2. Set Delegate Name: `ec2-docker-delegate`
3. Set Tags: `linux-amd64`
4. Copy the command and run on EC2:

```bash
docker run -d --cpus=1 --memory=2g \
  -e DELEGATE_NAME=ec2-docker-delegate \
  -e NEXT_GEN="true" \
  -e DELEGATE_TYPE="DOCKER" \
  -e ACCOUNT_ID=YOUR_ACCOUNT_ID \
  -e DELEGATE_TOKEN=YOUR_TOKEN \
  -e DELEGATE_TAGS="linux-amd64" \
  -e MANAGER_HOST_AND_PORT=https://app.harness.io \
  us-docker.pkg.dev/gar-prod-setup/harness-public/harness/delegate:latest
```

5. Wait 2 minutes
6. Check Harness UI → Delegates → `ec2-docker-delegate` → **Connected** ✅

---

## Step 5: Install Harness Docker Runner on EC2

```bash
# Download the runner binary
curl -L -o harness-docker-runner https://github.com/harness/harness-docker-runner/releases/download/v0.1.25/harness-docker-runner-linux-amd64

# Make it executable
chmod +x harness-docker-runner

# Run it in background
nohup ./harness-docker-runner server > runner.log 2>&1 &

# Verify it's running on port 3000
curl http://localhost:3000/healthz
```

If you see a response → Runner is ready! ✅

---

## Step 6: Install Auto-Upgrade (Optional)

```bash
docker run -d --cpus=0.1 --memory=100m \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e ACCOUNT_ID=YOUR_ACCOUNT_ID \
  -e MANAGER_HOST_AND_PORT=https://app.harness.io \
  -e UPGRADER_WORKLOAD_NAME=ec2-docker-delegate \
  -e UPGRADER_TOKEN=YOUR_TOKEN \
  -e CONTAINER_STOP_TIMEOUT=3600 \
  -e SCHEDULE="0 */1 * * *" harness/upgrader:latest
```

---

## Step 7: Create Rollback Directory on EC2

```bash
# Create the directory where rollback data is stored
mkdir -p /tmp/harness

# Check it exists
ls /tmp/harness/

# After first successful pipeline run, this file will appear:
# /tmp/harness/last-successful-tag.txt
```

---

## Step 8: Verify Everything Running on EC2

```bash
# Check Docker containers
docker ps

# Expected output:
# CONTAINER ID   IMAGE                    STATUS       NAMES
# xxxx           harness/delegate         Up 5 min     (delegate)
# yyyy           harness/upgrader         Up 3 min     (upgrader)

# Check runner process
ps aux | grep harness-docker-runner

# Check rollback directory
ls -la /tmp/harness/
```

3 things running: Delegate + Upgrader + Runner ✅

---

## Step 9: Push Go Code to GitHub

On your laptop:
```bash
cd Harness-CI-CD-Zero-to-Hero
git add .
git commit -m "Episode 3: Go project with Docker Delegate + auto rollback"
git push origin master
```

---

## Step 10: Create Pipeline in Harness

1. Pipelines → Import from Git
2. Connector: Github
3. Repo: Harness-CI-CD-Zero-to-Hero
4. Branch: master
5. YAML Path: Episode-03/go-project/.harness/pipeline-docker-delegate.yaml
6. Click Import

---

## Step 11: Run the Pipeline!

1. Click Run
2. Branch: master
3. Click Run Pipeline
4. Watch 7 steps:
   - Save Rollback Data ✅
   - Run Tests ✅
   - Build App ✅
   - Verify App ✅
   - Build and Push to Docker Hub ✅
   - Update Rollback Data ✅
   - Pipeline Complete ✅

---

## Step 12: Check Rollback Data on EC2

After pipeline succeeds, SSH to EC2 and check:

```bash
ssh -i harness-key.pem ubuntu@YOUR-EC2-IP

# See the last successful tag
cat /tmp/harness/last-successful-tag.txt
# Output: 1 (or whatever your build number is)
```

This file is updated every time a build succeeds. If next build fails, rollback reads this file.

---

## Step 13: Test the Rollback (Break the Code on Purpose!)

To test rollback, break the test on purpose:

1. Edit `Episode-03/go-project/main_test.go`
2. Change:
   ```go
   if resp.Message != "Hello from Harness CI/CD Course!" {
   ```
   To:
   ```go
   if resp.Message != "WRONG MESSAGE" {
   ```
3. Push and run pipeline
4. Test will FAIL → Automatic rollback triggers!
5. You'll see in logs:
   ```
   ⚠️ PIPELINE FAILED — ROLLING BACK!
   Failed tag: 2 (deleted)
   Stable tag: 1 (still available)
   'latest' points to: 1
   ROLLBACK COMPLETE!
   ```
6. Check Docker Hub → tag 2 is gone, tag 1 + latest still there
7. Fix the test back, push again → Build #3 succeeds

---

## Expected Output (Success)

Step 2 (Run Tests):
```
=== PASS: TestHomeHandler (0.00s)
=== PASS: TestHealthHandler (0.00s)
=== All Tests Passed! ===
```

Step 4 (Verify App):
```
{"message":"Hello from Harness CI/CD Course!","episode":3,"language":"Go",...}
{"status":"healthy"}
=== App Verified! ===
```

Step 7 (Complete):
```
=========================================
  PIPELINE COMPLETE!
  Image: yaswanth111/go-harness-app:1
  Image: yaswanth111/go-harness-app:latest
  Rollback tag saved: 1
  If next build fails → auto rollback to this version
=========================================
```

---

## Expected Output (Failure + Rollback)

```
=========================================
  ⚠️ PIPELINE FAILED — ROLLING BACK!
=========================================

Previous successful tag: 1
Failed build tag: 2

--- Deleting failed image tag 2 from Docker Hub ---
Failed tag 2 deleted
--- Restoring 'latest' tag to previous version: 1 ---
Previous image 1 is still on Docker Hub as the stable version

=========================================
  ROLLBACK COMPLETE!
  Failed tag: 2 (deleted)
  Stable tag: 1 (still available)
  'latest' points to: 1
=========================================
```

---

## Where is the Rollback File on EC2?

```
Location: /tmp/harness/last-successful-tag.txt

To find it:
ssh -i harness-key.pem ubuntu@YOUR-EC2-IP
cat /tmp/harness/last-successful-tag.txt

Example content: 3
(means build #3 was the last successful one)

This file is:
- Created on first successful build
- Updated after every successful build
- Read during rollback to know which version to keep
- Stored on EC2 in /tmp/harness/ directory
```

---

## Project Structure

```
Episode-03/
├── README.md                              ← This file
└── go-project/
    ├── main.go                            ← Go web app (/ and /health)
    ├── main_test.go                       ← 2 unit tests
    ├── go.mod                             ← Go module
    ├── Dockerfile                         ← Multi-stage Docker build
    └── .harness/
        └── pipeline-docker-delegate.yaml  ← CI pipeline (7 steps + rollback)
```

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "Failed to connect to port 3000" | Runner not running. SSH to EC2, run: `./harness-docker-runner server &` |
| "No eligible delegates" | Check delegate has tag `linux-amd64` |
| "Connector not found" | Check connector name matches exactly in YAML |
| Rollback file not found | First run creates it. Check `/tmp/harness/` on EC2 |
| EC2 SSH timeout | Check security group allows port 22 from your IP |
| Docker pull fails on EC2 | Check EC2 has outbound internet (security group) |

---

## Cost

- EC2 `t2.medium`: ~$0.046/hour = ~$1.10/day
- **Stop the instance when not using!**
- AWS Console → EC2 → Select instance → Instance State → Stop

---

## Key Takeaways

1. Docker Delegate = runs on any machine with Docker (EC2, laptop, anywhere)
2. Docker Runner = separate binary, listens on port 3000, executes pipeline containers
3. Rollback data stored in `/tmp/harness/last-successful-tag.txt` on EC2
4. If pipeline fails → automatic rollback deletes failed tag, keeps previous stable one
5. `latest` tag always points to last successful build
6. Go builds are fast (entire pipeline ~2-3 minutes)

---

> 🎬 Next Episode: [Episode 4 - Build Your First Enterprise CI Pipeline](../Episode-04/README.md)
