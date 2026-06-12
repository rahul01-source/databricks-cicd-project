# Databricks CI/CD Project

End-to-end CI/CD pipeline for Databricks Medallion Architecture using Asset Bundles (DABs) and GitHub Actions.

## 🏗️ Architecture

```
Bronze Layer → Silver Layer → Gold Layer
(Raw Data)     (Cleansed)     (Business Metrics)
```

## 📁 Project Structure

```
databricks-cicd-project/
├── README.md                    # This file
├── bundles/                     # 🎯 DAB configuration folder
│   ├── databricks.yml           # Main DAB config
│   ├── .gitignore              # Git ignore rules
│   ├── README.md               # Bundle documentation
│   ├── SETUP.md                # Setup instructions
│   ├── .github/
│   │   └── workflows/          # CI/CD pipelines
│   │       ├── deploy-dev.yml
│   │       ├── deploy-uat.yml
│   │       └── deploy-prod.yml
│   ├── resources/
│   │   └── jobs.yml            # Job definitions
│   ├── src/
│   │   ├── bronze/             # Bronze notebooks
│   │   ├── silver/             # Silver notebooks
│   │   └── gold/               # Gold notebooks
│   └── tests/                  # Unit tests (optional)
```

## 🚀 Quick Start

### 1. Configure GitHub

**Add Secrets** (Settings → Secrets and variables → Actions):
- `DATABRICKS_HOST`: Your workspace URL
- `DATABRICKS_CLIENT_ID`: Service principal client ID
- `DATABRICKS_CLIENT_SECRET`: Service principal secret

**Create Environments** (Settings → Environments):
- `uat` (no approval)
- `production` (with approval)

### 2. Copy Notebooks

Export your notebooks from workspace and place in:
- `bundles/src/bronze/01_ingest_raw_transactions.py`
- `bundles/src/silver/02_cleanse_transactions.py`
- `bundles/src/gold/03_business_metrics.py`

### 3. Deploy

```bash
# Commit to feature branch
git checkout -b feature_01
git add bundles/
git commit -m "feat: add DAB configuration"
git push origin feature_01

# Create PR and merge to dev
# → Auto-deploys to Dev environment!
```

## 🔄 Branch Strategy

```
dev branch  →  uat branch  →  main branch
(Dev)          (UAT)          (Production)
Auto           Auto           Manual Approval
```

## 📚 Documentation

- [Bundle README](bundles/README.md) - Complete bundle documentation
- [Setup Guide](bundles/SETUP.md) - Step-by-step setup instructions
- [Databricks DABs Docs](https://docs.databricks.com/dev-tools/bundles/)

## 🎯 Environments

| Environment | Branch | Job Name | Deployment |
|-------------|--------|----------|------------|
| **Dev** | `dev` | `dev_medallion_etl_pipeline` | Automatic |
| **UAT** | `uat` | `uat_medallion_etl_pipeline` | Automatic |
| **Prod** | `main` | `prod_medallion_etl_pipeline` | Manual Approval |

## ✅ Next Steps

1. [ ] Configure GitHub secrets
2. [ ] Create GitHub environments
3. [ ] Copy notebooks to `bundles/src/`
4. [ ] Commit to `feature_01` branch
5. [ ] Create Pull Request to `dev`
6. [ ] Merge and watch first deployment!

See [bundles/SETUP.md](bundles/SETUP.md) for detailed instructions.

---

**Built with ❤️ using Databricks Asset Bundles**
