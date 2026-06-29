# 🚀 How to Deploy Hello World App in Harness (With Screenshots Flow)

## The Complete Flow (What You'll Do)

```
Push Code to GitHub
       ⬇️
Login to Harness → Choose "Continuous Integration"
       ⬇️
Create Pipeline → Select "Remote" → Choose GitHub Repo
       ⬇️
Select Connector (GitHub) → Select Repository → Branch
       ⬇️
Click "Start" → Pipeline Studio Opens
       ⬇️
Click "Add Stage" → Name it "dev" → Configure Codebase
       ⬇️
Select Infrastructure → "Cloud" → Linux
       ⬇️
Go to Execution → "Add Step" → Choose "Run"
       ⬇️
Write build commands → Save → Click "Run Pipeline"
       ⬇️
Select Branch → Click "Run Pipeline" → Watch it BUILD! ✅
```

---

## STEP 1: Push This Code to GitHub

```bash
cd hello-world-app
git init
git add .
git commit -m "Hello World - Harness Episode 1"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/hello-world-harness.git
git push -u origin main
```

---

## STEP 2: Login to Harness & Choose Module

```
1. Go to → https://app.harness.io
2. Login with your account
3. You see the module selection page:
```

```
┌─────────────────────────────────────────────────────────────────┐
│  Supercharge every step of software development and delivery     │
│  Choose your starting use case:                                  │
│                                                                  │
│  ┌───────────────────┐ ┌───────────────────┐ ┌──────────────┐  │
│  │ ◆ Continuous      │ │ ● Continuous      │ │ ● Code       │  │
│  │   Integration     │ │   Delivery &      │ │   Repository │  │
│  │                   │ │   GitOps          │ │              │  │
│  │ [Get Started] ←───┤ │ [Get Started]     │ │ [Get Started]│  │
│  └───────────────────┘ └───────────────────┘ └──────────────┘  │
│           ↑                                                      │
│     CLICK THIS ONE                                               │
│                                                                  │
│  ┌───────────────────┐ ┌───────────────────┐ ┌──────────────┐  │
│  │ ● Cloud Cost      │ │ ● Feature Flags   │ │ ● Service    │  │
│  │   Management      │ │                   │ │   Reliability│  │
│  │ [Get Started]     │ │ [Get Started]     │ │ [Get Started]│  │
│  └───────────────────┘ └───────────────────┘ └──────────────┘  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

→ Click "Get Started" under Continuous Integration
```

---

## STEP 3: You Land on CI Overview Dashboard

```
┌──────────────────────────────────────────────────────────────────┐
│  Continuous Integration                                           │
│  ─────────────────────                                           │
│  PROJECT: Default Project                                        │
│                                                                   │
│  Left Sidebar:          │  Main Area:                            │
│  ┌────────────────┐     │  ┌──────────────────────────────────┐ │
│  │ ◉ Overview     │     │  │  Overview                         │ │
│  │ ○ Pipelines    │     │  │                                   │ │
│  │ ○ Builds       │     │  │  Builds Health     Builds         │ │
│  │ ○ Get Started  │     │  │  Total: 0          (empty chart)  │ │
│  │                │     │  │                                   │ │
│  │ ○ Project      │     │  │  ┌─────────────────────────────┐ │ │
│  │   Settings     │     │  │  │ Set up a Pipeline and run   │ │ │
│  │ ○ Account      │     │  │  │ your first build!           │ │ │
│  │   Settings     │     │  │  │                             │ │ │
│  │ ○ Organization │     │  │  │ [Create a Pipeline] ← CLICK│ │ │
│  │   Settings     │     │  │  └─────────────────────────────┘ │ │
│  └────────────────┘     │  └──────────────────────────────────┘ │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘

→ Click "Create a Pipeline" button (or go to Pipelines → + Create)
```

---

## STEP 4: Create New Pipeline Dialog

```
┌──────────────────────────────────────────────────────────┐
│  Create new Pipeline                               ✕     │
│                                                          │
│  Name ⓘ                          Id ⓘ: First ✏️        │
│  ┌──────────────────────────────────┐                    │
│  │  First                            │ ← TYPE YOUR NAME  │
│  └──────────────────────────────────┘                    │
│                                                          │
│  Description (optional) ✏️                               │
│  Tags (optional) ✏️                                      │
│                                                          │
│  How do you want to set up your pipeline?                │
│                                                          │
│  ┌─────────────────────┐  ┌──────────────────────────┐  │
│  │  Inline             │  │  ✅ Remote               │  │
│  │  Pipeline is stored │  │  Pipeline is stored in   │  │
│  │  in Harness         │  │  your Git repository     │  │
│  └─────────────────────┘  └──────────────────────────┘  │
│                                   ↑ SELECT THIS          │
│                                                          │
│  Where is your Git Repository?                           │
│                                                          │
│  ┌──────────────────────┐  ┌─────────────────────────┐  │
│  │ Harness Code         │  │ ✅ Third-party Git      │  │
│  │ Repository            │  │    provider             │  │
│  └──────────────────────┘  └─────────────────────────┘  │
│                                   ↑ SELECT THIS          │
│                                                          │
│  Git Connector ⓘ                                        │
│  ┌──────────────────────────────────────┐               │
│  │  - Select -                    ▼  ✏️  │               │
│  └──────────────────────────────────────┘               │
│     ↑ If empty, click ✏️ to CREATE a new connector       │
│     (See "HOW TO CONNECT GIT" section below)            │
│                                                          │
│  Repository ⓘ                                           │
│  ┌──────────────────────────────────────┐               │
│  │  YOUR-USERNAME/hello-world-harness ▼  │               │
│  └──────────────────────────────────────┘               │
│     ↑ Select your GitHub repo from dropdown              │
│                                                          │
│  Git Branch ⓘ                                           │
│  ┌──────────────────────────────────────┐               │
│  │  main (or master)              ▼     │               │
│  └──────────────────────────────────────┘               │
│                                                          │
│  YAML Path ⓘ                                            │
│  .harness/pipeline.yaml  (auto-filled)                   │
│                                                          │
│  ☐ Start with Template                                   │
│                                                          │
│  [Start]  [Cancel]                                       │
│                                                          │
└──────────────────────────────────────────────────────────┘

→ Fill all fields → Click "Start"
```

---

## 📌 HOW TO CONNECT GIT (Create GitHub Connector — FIRST TIME ONLY)

**You need to do this BEFORE creating a pipeline (or during pipeline creation when it asks for connector)**

### Method 1: Create Connector DURING Pipeline Creation

```
When you see "Git Connector" dropdown and it's empty:

1. Click the pencil icon ✏️ next to the dropdown
2. Click "+ New Connector"
3. Connector type screen appears (blue screen):

┌─────────────────────────────────────────────────────────────────┐
│                                                          ✕       │
│  Select your Connector type                                      │
│  Start by selecting your connector type                          │
│                                                                  │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│  │  GitHub   │ │Bitbucket │ │Azure Repos│ │  GitLab  │          │
│  │Connector  │ │Connector │ │Connector  │ │Connector │          │
│  │   ↑       │ │          │ │           │ │          │          │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘          │
│  CLICK THIS                                                      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Method 2: Create Connector from Project Settings (BEFORE pipeline)

```
1. Left sidebar → "Project Settings"
2. Under "Project-level Resources" → Click "Connectors"
3. Click "+ New Connector"
4. Under "Code Repositories" → Click "GitHub"
```

---

### Fill in GitHub Connector Details (Step by Step):

#### Screen 1: Overview

```
┌──────────────────────────────────────────────────────────┐
│  Overview                                                 │
│                                                          │
│  Name ⓘ                                                 │
│  ┌──────────────────────────────────┐                   │
│  │  github-connector                 │ ← TYPE THIS      │
│  └──────────────────────────────────┘                   │
│                                                          │
│  Id: github_connector (auto-generated)                   │
│  Description: (optional)                                 │
│  Tags: (optional)                                        │
│                                                          │
│  URL Type:                                               │
│  ● Account  ○ Repository                                │
│  ↑ SELECT "Account" (connects to ALL your repos)        │
│                                                          │
│  Connection Type:                                        │
│  ● HTTP  ○ SSH                                          │
│  ↑ SELECT "HTTP"                                        │
│                                                          │
│  GitHub Account URL:                                     │
│  ┌──────────────────────────────────────────────────┐   │
│  │  https://github.com/YOUR-GITHUB-USERNAME          │   │
│  └──────────────────────────────────────────────────┘   │
│  ↑ Replace YOUR-GITHUB-USERNAME with your actual name   │
│  Example: https://github.com/yaswanthreddy                │
│                                                          │
│  [Continue >]                                            │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

#### Screen 2: Credentials

```
┌──────────────────────────────────────────────────────────┐
│  Credentials                                              │
│                                                          │
│  Authentication:                                         │
│  ● Username and Token  ○ OAuth  ○ GitHub App             │
│  ↑ SELECT "Username and Token"                           │
│                                                          │
│  Username:                                               │
│  ┌──────────────────────────────────────────────────┐   │
│  │  YOUR-GITHUB-USERNAME                             │   │
│  └──────────────────────────────────────────────────┘   │
│                                                          │
│  Personal Access Token: ← This is a SECRET              │
│  ┌──────────────────────────────────────────────────┐   │
│  │  - Select -                         ▼   [+ New]  │   │
│  └──────────────────────────────────────────────────┘   │
│                                        ↑                 │
│                               Click "+ New Secret"        │
│                                                          │
│  ┌──────────────────────────────────────────────┐       │
│  │  Create New Secret                            │       │
│  │                                               │       │
│  │  Secret Name: github-pat                      │       │
│  │  Secret Value: ghp_xxxxxxxxxxxxxxxxxxxx       │       │
│  │               ↑ PASTE YOUR GITHUB TOKEN HERE  │       │
│  │                                               │       │
│  │  [Save]                                       │       │
│  └──────────────────────────────────────────────┘       │
│                                                          │
│  Enable API access: ✅ (check this box)                  │
│  API Authentication: Same credentials                    │
│                                                          │
│  [Continue >]                                            │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

#### Screen 3: Connect to Provider

```
┌──────────────────────────────────────────────────────────┐
│  Connect to the provider                                  │
│                                                          │
│  Select connectivity mode:                               │
│                                                          │
│  ● Connect through Harness Platform    ← SELECT THIS    │
│    (easiest, no delegate needed)                         │
│                                                          │
│  ○ Connect through a Harness Delegate                    │
│    (for private/self-hosted GitHub)                       │
│                                                          │
│  [Save and Continue]                                     │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

#### Screen 4: Connection Test

```
┌──────────────────────────────────────────────────────────┐
│  Connection Test                                          │
│                                                          │
│  ✅ Connection Successful                                │
│                                                          │
│  Harness was able to connect to GitHub!                  │
│                                                          │
│  [Finish]                                                │
│                                                          │
└──────────────────────────────────────────────────────────┘

If you see ❌ Connection Failed:
  → Check your GitHub username (case-sensitive!)
  → Check your PAT token (copy again from GitHub)
  → Make sure PAT has "repo" scope
  → Make sure PAT is not expired
```

---

### 🔑 How to Get GitHub Personal Access Token (PAT)

```
┌──────────────────────────────────────────────────────────┐
│  STEP-BY-STEP TO CREATE GITHUB PAT:                       │
│                                                          │
│  1. Go to → https://github.com/settings/tokens           │
│                                                          │
│     OR: GitHub → Click avatar (top-right) → Settings    │
│     → Developer settings (very bottom of left sidebar)  │
│     → Personal access tokens → Tokens (classic)         │
│                                                          │
│  2. Click "Generate new token" → "Generate new token     │
│     (classic)"                                           │
│                                                          │
│  3. Fill in:                                             │
│     ┌────────────────────────────────────────────────┐  │
│     │  Note: harness-cicd-access                      │  │
│     │  Expiration: 90 days (or No expiration)         │  │
│     │                                                 │  │
│     │  Scopes (check these boxes):                    │  │
│     │  ✅ repo (Full control of private repositories) │  │
│     │     ✅ repo:status                              │  │
│     │     ✅ repo_deployment                          │  │
│     │     ✅ public_repo                              │  │
│     │     ✅ repo:invite                              │  │
│     │  ✅ admin:repo_hook (for webhooks/triggers)     │  │
│     │     ✅ write:repo_hook                          │  │
│     │     ✅ read:repo_hook                           │  │
│     └────────────────────────────────────────────────┘  │
│                                                          │
│  4. Click "Generate token" (green button at bottom)      │
│                                                          │
│  5. ⚠️  COPY THE TOKEN NOW!                              │
│     It starts with: ghp_xxxxxxxxxxxxxxxxxxxxxxxx         │
│     You will NEVER see it again after leaving this page!│
│                                                          │
│  6. Save it somewhere safe (notepad, password manager)   │
│     Then paste it into Harness when creating the secret │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

### ✅ After Connector is Created

```
Now when you go back to "Create Pipeline" dialog:

Git Connector ⓘ
┌──────────────────────────────────────┐
│  github-connector   ● PROJECT   ▼   │  ← Your connector shows here!
└──────────────────────────────────────┘

Repository ⓘ
┌──────────────────────────────────────┐
│  hello-world-harness            ▼   │  ← Your repos auto-load!
└──────────────────────────────────────┘

Git Branch ⓘ
┌──────────────────────────────────────┐
│  main                           ▼   │
└──────────────────────────────────────┘

→ Now you can proceed with pipeline creation! 🎉
```

---

## STEP 5: If You Don't Have a Connector Yet — Create One

```
When you click the connector dropdown, if empty:
→ Click the pencil icon ✏️ next to connector field
→ "Select your Connector type" screen appears:

┌─────────────────────────────────────────────────────────┐
│                                                          │
│  Select your Connector type                              │
│  Start by selecting your connector type                  │
│                                                          │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │
│  │  GitHub   │ │Bitbucket │ │Azure Repos│ │  GitLab  │  │
│  │Connector  │ │Connector │ │Connector  │ │Connector │  │
│  │    ↑      │ │          │ │           │ │          │  │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘  │
│  CLICK THIS                                              │
│                                                          │
└─────────────────────────────────────────────────────────┘

→ Click "GitHub Connector"
→ Name: gitconnector
→ URL: https://github.com/YOUR-USERNAME
→ Authentication: Personal Access Token
→ Username: YOUR-GITHUB-USERNAME
→ Token: Create new secret → paste your PAT
→ Save and Continue → Test → ✅ Success
```

**How to get GitHub PAT (if you don't have one):**
```
1. GitHub.com → Click your avatar (top-right)
2. Settings → Developer settings (bottom of left sidebar)
3. Personal access tokens → Tokens (classic)
4. Generate new token (classic)
5. Name: "harness-access"
6. Scopes: ✅ repo (check the full "repo" checkbox)
7. Click "Generate token"
8. COPY THE TOKEN (you can't see it again!)
```

---

## STEP 6: Pipeline Studio Opens — Add a Stage

```
┌──────────────────────────────────────────────────────────────────┐
│  Pipeline Studio                                                  │
│  ─────────────────                                               │
│  ○ First ✏️  │ express-hello-... │ ⚡ master │ [Visual] [YAML]  │
│                                                                   │
│  ┌──────────────────────────────────────────────────────┐        │
│  │                                                       │        │
│  │   ▶○────── [+] ──────○■                              │        │
│  │              ↑                                        │        │
│  │         Add Stage ← CLICK THE [+]                    │        │
│  │                                                       │        │
│  └──────────────────────────────────────────────────────┘        │
│                                                                   │
│  Right sidebar: Variables | Notify | Flow Control | Codebase     │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘

→ Click the [+] "Add Stage"
→ Choose "Build" (the CI stage type)
```

---

## STEP 7: Configure the Stage

```
┌──────────────────────────────────────────────────────────┐
│  ⓘ About your Stage                                      │
│                                                          │
│  Stage Name ⓘ              Id ⓘ  dm ✏️                 │
│  ┌──────────────────────────────────────┐               │
│  │  dev                                  │ ← TYPE "dev"  │
│  └──────────────────────────────────────┘               │
│                                                          │
│  Description (optional) ✏️                               │
│  Tags (optional) ✏️                                      │
│                                                          │
│  ● Clone Codebase ← KEEP THIS ON (toggle is blue)       │
│                                                          │
│  ⚙️ Configure Codebase                                   │
│  Connect Harness with your code repo.                    │
│                                                          │
│  Select Git Provider:                                    │
│  ┌──────────────────────┐  ┌──────────────────────────┐ │
│  │ Harness Code          │  │ ✅ Third-party Git       │ │
│  │ Repository             │  │    provider             │ │
│  └──────────────────────┘  └──────────────────────────┘ │
│                                    ↑ SELECT THIS         │
│                                                          │
│  Connector ⓘ                                            │
│  ┌──────────────────────────────────────┐               │
│  │  gitconnector              ▼    ✏️    │               │
│  └──────────────────────────────────────┘               │
│                                                          │
│  Repository Name ⓘ                                      │
│  ┌──────────────────────────────────────┐               │
│  │  hello-world-harness                  │               │
│  └──────────────────────────────────────┘               │
│                                                          │
│  [Set Up Stage] ← CLICK THIS                            │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## STEP 8: Select Infrastructure

```
┌──────────────────────────────────────────────────────────────────┐
│  Pipeline: First  │ express-hello-... │ master                    │
│                                                                   │
│  ▶○──── [dev] ──── [+] ────○■                                   │
│            ↑                                                      │
│     Your stage (blue = configured)                               │
│                                                                   │
│  ● Overview  > ▲ Infrastructure  > ▲ Execution  > □ Advanced    │
│                       ↑ YOU ARE HERE                              │
│                                                                   │
│  Infrastructure                                                   │
│  Select the infrastructure where you want your builds to run     │
│                                                                   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                      │
│  │  ✅ Cloud │  │Kubernetes│  │  Local   │                      │
│  │    ↑      │  │          │  │          │                      │
│  └──────────┘  └──────────┘  └──────────┘                      │
│  SELECT THIS                                                      │
│  (Free, no delegate needed!)                                     │
│                                                                   │
│  Platform                                                         │
│  Select the Operating System ⓘ                                   │
│  ┌────────────────────────────────┐                              │
│  │  Linux                    ▼    │ ← KEEP AS LINUX              │
│  └────────────────────────────────┘                              │
│                                                                   │
│  [< Back]  [Continue >] ← CLICK CONTINUE                        │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

## STEP 9: Add Execution Step (The Build Command)

```
┌──────────────────────────────────────────────────────────────────┐
│  ● Overview  > Infrastructure  > ▲ Execution  > □ Advanced      │
│                                       ↑ YOU ARE HERE             │
│                                                                   │
│  You see the execution canvas:                                   │
│                                                                   │
│  ┌─────────────────────────────────────────┐                    │
│  │                                          │                    │
│  │    ──○── [+] ──○──                      │                    │
│  │          Add Step ← CLICK HERE           │                    │
│  │                                          │                    │
│  └─────────────────────────────────────────┘                    │
│                                                                   │
│  When you click [+] you see options:                             │
│  ┌─────────────────┐                                            │
│  │  ● Add Step      │ ← CLICK THIS                              │
│  │  ● Add Step Group│                                            │
│  │  ● Use Template  │                                            │
│  └─────────────────┘                                            │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘

→ Click "Add Step"
→ Step Library opens on the right side:
```

```
┌─────────────────────────────────────────────────────────┐
│  Step Library                         Q Search           │
│                                                          │
│  Build (17)        ← LOOK HERE                          │
│  ┌──────┐ ┌──────────┐ ┌─────────────────┐            │
│  │ Run  │ │Background│ │Build and Push   │            │
│  │  ↑   │ │          │ │to ECR           │            │
│  └──────┘ └──────────┘ └─────────────────┘            │
│  CLICK                                                   │
│  "Run"   ┌─────────────────┐ ┌─────────────────┐      │
│           │Build and Push   │ │Build and Push   │      │
│           │to GCR           │ │to GAR           │      │
│           └─────────────────┘ └─────────────────┘      │
│                                                          │
│  ... more steps available ...                            │
│                                                          │
│  Library (right side):                                   │
│  Build (17) | Artifacts (8) | Security Tests (7)        │
│  Built-in Scanners (5) | Container (12)                 │
│                                                          │
└─────────────────────────────────────────────────────────┘

→ Click "Run" (first option under Build)
```

---

## STEP 10: Configure the Run Step

```
┌──────────────────────────────────────────────────────────┐
│  Configure Run Step                                       │
│                                                          │
│  Name:                                                   │
│  ┌──────────────────────────────────┐                   │
│  │  Build and Run Hello World        │                   │
│  └──────────────────────────────────┘                   │
│                                                          │
│  Shell:                                                  │
│  ┌──────────────────────────────────┐                   │
│  │  Sh                          ▼   │                   │
│  └──────────────────────────────────┘                   │
│                                                          │
│  Command:                                                │
│  ┌──────────────────────────────────────────────────┐   │
│  │  echo "=== Building Hello World ==="              │   │
│  │  mvn clean package -q                             │   │
│  │  echo ""                                          │   │
│  │  echo "=== Running the App ==="                   │   │
│  │  java -jar target/hello-world-1.0.0.jar           │   │
│  │  echo ""                                          │   │
│  │  echo "=== Pipeline Success! ==="                 │   │
│  └──────────────────────────────────────────────────┘   │
│                                                          │
│  Container Registry: (leave empty for Cloud infra)       │
│  Image: (leave empty — Cloud has Java + Maven)           │
│                                                          │
│  [Apply Changes] ← CLICK THIS                           │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## STEP 11: Save and Run the Pipeline

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                   │
│  Top-right corner of Pipeline Studio:                            │
│                                                                   │
│  [Save ▼]  [Run] ← FIRST Save, THEN Run                        │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘

→ Click "Save" first
→ Then click "Run"
→ Run Pipeline dialog appears:
```

```
┌──────────────────────────────────────────────────────────┐
│  Run Pipeline ⓘ  │ All Stages ▼ │ [Visual] [YAML]       │
│                                                          │
│  📦 express-hello-...  │ ⚡ master-patch-1  ● ▼         │
│                                                          │
│  Use existing Input Sets ⓘ  ○                           │
│                                                          │
│  CI Codebase                                             │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Build Type                                       │   │
│  │  ● Git Branch  ○ Git Tag  ○ Git Pull Request     │   │
│  │                                                   │   │
│  │  Branch Name                                      │   │
│  │  ┌───────────────────────────────────────┐       │   │
│  │  │  main (or master)              ✔️      │       │   │
│  │  └───────────────────────────────────────┘       │   │
│  └──────────────────────────────────────────────────┘   │
│                                                          │
│  ☑ Skip preflight check                                  │
│  ○ Notify only me about execution status                 │
│                                                          │
│  [Run Pipeline] ← CLICK THIS!     [Cancel]              │
│                                                          │
└──────────────────────────────────────────────────────────┘

→ Select branch: main (or master)
→ Click "Run Pipeline" 🚀
```

---

## STEP 12: Watch It Build! ✅

```
┌──────────────────────────────────────────────────────────────────┐
│  CI Overview                                                      │
│                                                                   │
│  Builds Health            │  Active Builds (1)                   │
│  Total Builds: 1          │                                       │
│  Successful: 0            │  First              [RUNNING]        │
│  Failed: 0                │  ⚡ master                            │
│                           │  ● Build and Run...  c7f5c8 🔗       │
│  Repositories (1)         │  ⏱️ 1 minute ago  ⏳ 1m 14s          │
│  express-hello-world      │                                       │
│  BUILDS: 1  SUCCESS: 0%   │                                       │
│                           │                                       │
└──────────────────────────────────────────────────────────────────┘

Wait ~1-2 minutes...

When it finishes:
  Active Builds (0) → No Active Builds found
  Build status shows: ✅ SUCCESS (green)

→ Click on the build to see logs
→ You'll see your "Hello from Harness CI/CD! 🚀" output!
```

---

## STEP 13: Verify — Projects View

```
┌──────────────────────────────────────────────────────────────────┐
│  Accounts │ Organizations │ Projects                              │
│                                                                   │
│  Q Search your project or id │ All Organiza... ▼ │ Manage Projects│
│                                                                   │
│  Total: 1                    Sort: Last Modified ▼                │
│                                                                   │
│  ┌─────────────────────────────────────┐                         │
│  │  ─── Default Project ───            │                         │
│  │  Id: default_project                 │                         │
│  │                                      │                         │
│  │  Admins (1)   Collaborators          │                         │
│  │  👤             👤                   │                         │
│  │                                      │                         │
│  │  Modules: ● ● ● (CI, CD, etc.)     │                         │
│  │                                      │                         │
│  └─────────────────────────────────────┘                         │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘

🎉 YOUR FIRST HARNESS PIPELINE IS COMPLETE!
```

---

## 📝 Quick Summary (What You Just Did)

```
STEP 1:  Pushed code to GitHub                    (30 sec)
STEP 2:  Logged into Harness                      (10 sec)
STEP 3:  Clicked "Create a Pipeline"              (5 sec)
STEP 4:  Selected "Remote" + GitHub repo          (30 sec)
STEP 5:  Created GitHub connector (if needed)     (2 min)
STEP 6:  Clicked "Start" → Pipeline Studio        (5 sec)
STEP 7:  Added stage "dev" + configured codebase  (30 sec)
STEP 8:  Selected "Cloud" infrastructure + Linux  (10 sec)
STEP 9:  Added "Run" step                         (10 sec)
STEP 10: Typed build commands                     (30 sec)
STEP 11: Saved + Clicked "Run"                    (10 sec)
STEP 12: Watched it build → SUCCESS ✅            (1-2 min)
─────────────────────────────────────────────────────────
TOTAL TIME: ~5-6 minutes (first time)
NEXT TIME:  ~30 seconds (just click Run!)
```

---

## ❓ Troubleshooting

| Problem | Solution |
|---------|----------|
| Connector not showing repos | Check PAT has `repo` scope |
| "Repository not found" | Make sure repo name matches exactly |
| "mvn: command not found" | Make sure you selected "Cloud" infra (not Local) |
| Build fails | Check pom.xml is in root of your repo |
| Branch not found | Try `master` instead of `main` (or vice versa) |
| Pipeline stuck on "Initializing" | Wait 30-60 sec, Cloud infra needs to spin up |

---

## ⏭️ What's Next?

```
✅ Episode 1: DONE! You ran your first pipeline!
📍 Episode 2: Organize (Org, Projects, RBAC, Secrets)
📍 Episode 3: Delegate + Connectors (GitHub, Docker, AWS, K8s)
📍 Episode 4: REAL CI pipeline (tests, Docker build, push image)
```

You proved Harness works. Now we build enterprise-grade stuff! 💪
