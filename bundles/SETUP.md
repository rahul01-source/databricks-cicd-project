# 🚀 Setup Guide

## Step-by-Step Setup Instructions

### Phase 1: Databricks Configuration ✅ (Already Done)

- [x] Service Principal created: `cicd-deployment-sp`
- [x] OAuth secrets generated
- [ ] Permissions granted (if needed):
  ```sql
  GRANT USAGE ON CATALOG main TO `cicd-deployment-sp`;
  GRANT CREATE SCHEMA ON CATALOG main TO `cicd-deployment-sp`;
  GRANT ALL PRIVILEGES ON SCHEMA main.default TO `cicd-deployment-sp`;
  ```

### Phase 2: GitHub Configuration

#### 2.1 Add GitHub Secrets

Go to: **Settings → Secrets and variables → Actions → New repository secret**

Add these three secrets:

| Secret Name | Value | Status |
|-------------|-------|--------|
| `DATABRICKS_HOST` | `https://dbc-a4d64d52-0641.cloud.databricks.com` | ⏳ |
| `DATABRICKS_CLIENT_ID` | From service principal | ⏳ |
| `DATABRICKS_CLIENT_SECRET` | From service principal | ⏳ |

#### 2.2 Create GitHub Environments

Go to: **Settings → Environments**

Create two environments:

1. **`uat`**
   - No protection rules
   - Auto-deploy enabled

2. **`production`**
   - ✅ Required reviewers (add yourself)
   - ✅ Manual approval required

### Phase 3: Copy Your Notebooks

Copy your existing notebooks to the `src/` folder:

```bash
# From your workspace, copy notebooks to:
bundles/src/bronze/01_ingest_raw_transactions.py
bundles/src/silver/02_cleanse_transactions.py
bundles/src/gold/03_business_metrics.py
```

**Note**: Export notebooks as `.py` files (Source format) from Databricks.

### Phase 4: Test Locally (Optional)

```bash
# Install Databricks CLI
pip install databricks-cli

# Set credentials
export DATABRICKS_HOST="https://dbc-a4d64d52-0641.cloud.databricks.com"
export DATABRICKS_CLIENT_ID="<your-client-id>"
export DATABRICKS_CLIENT_SECRET="<your-client-secret>"

# Validate
cd bundles
databricks bundle validate -t dev

# Deploy to dev
databricks bundle deploy -t dev

# Run job
databricks bundle run -t dev medallion_etl_job
```

### Phase 5: Commit to Git

#### Option A: Via Databricks Repos UI

1. Click the **Git** icon in the left sidebar
2. Click **Commit**
3. Enter commit message: "feat: add bundles configuration"
4. Click **Commit & Push**
5. Select branch: `feature_01`

#### Option B: Via Command Line (if Git is configured)

```bash
cd /Workspace/Repos/rahulmhatre173@gmail.com/databricks-cicd-project

# Check current branch
git branch

# Create feature_01 branch if needed
git checkout -b feature_01

# Stage all files
git add bundles/

# Commit
git commit -m "feat: add DAB bundles configuration"

# Push to GitHub
git push origin feature_01
```

### Phase 6: Create Pull Request

1. Go to your GitHub repository
2. Click **Pull Requests**
3. Click **New Pull Request**
4. Base: `dev`, Compare: `feature_01`
5. Create Pull Request
6. After review, merge to `dev`

✅ **First deployment to Dev happens automatically!**

### Phase 7: Verify Deployment

1. Go to GitHub → **Actions** tab
2. Watch "Deploy to Dev" workflow
3. Check Databricks → **Workflows**
4. Find job: `dev_medallion_etl_pipeline`
5. Click **Run now** to test

## 🎯 Next Steps

After Dev deployment succeeds:

1. **Promote to UAT**:
   ```bash
   git checkout uat
   git merge dev
   git push origin uat
   ```

2. **Promote to Production**:
   ```bash
   git checkout main
   git merge uat
   git push origin main
   # Then approve in GitHub Actions
   ```

## ✅ Success Checklist

- [ ] GitHub secrets configured
- [ ] GitHub environments created (`uat`, `production`)
- [ ] Notebooks copied to `src/` folders
- [ ] Committed to `feature_01` branch
- [ ] Pull Request created and merged to `dev`
- [ ] First deployment to Dev successful
- [ ] Job `dev_medallion_etl_pipeline` created
- [ ] Job runs successfully
- [ ] Ready to promote to UAT and Production

## 🆘 Need Help?

See [README.md](README.md) for full documentation and troubleshooting.
