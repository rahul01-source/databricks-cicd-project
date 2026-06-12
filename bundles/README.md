# Databricks Asset Bundles (DABs) Configuration

This folder contains the complete CI/CD configuration for deploying the Medallion Architecture ETL pipeline across Dev, UAT, and Production environments.

## 📁 Structure

```
bundles/
├── databricks.yml              # Main DAB configuration
├── .gitignore                  # Git ignore rules
├── README.md                   # This file
├── .github/
│   └── workflows/              # GitHub Actions CI/CD
│       ├── deploy-dev.yml      # Dev deployment (dev branch)
│       ├── deploy-uat.yml      # UAT deployment (uat branch)
│       └── deploy-prod.yml     # Prod deployment (main branch)
├── resources/
│   └── jobs.yml                # Job definitions
├── src/
│   ├── bronze/                 # Bronze layer notebooks
│   ├── silver/                 # Silver layer notebooks
│   └── gold/                   # Gold layer notebooks
└── tests/                      # Unit tests (optional)
```

## 🚀 Quick Start

### Prerequisites

1. **Service Principal** created in Databricks
2. **GitHub Secrets** configured:
   - `DATABRICKS_HOST`
   - `DATABRICKS_CLIENT_ID`
   - `DATABRICKS_CLIENT_SECRET`
3. **GitHub Environments** created:
   - `uat` (no approval)
   - `production` (with approval)

### Deploy Locally (Testing)

```bash
# Install Databricks CLI
pip install databricks-cli

# Set environment variables
export DATABRICKS_HOST="https://dbc-a4d64d52-0641.cloud.databricks.com"
export DATABRICKS_CLIENT_ID="<your-client-id>"
export DATABRICKS_CLIENT_SECRET="<your-client-secret>"

# Validate bundle
databricks bundle validate -t dev

# Deploy to dev
databricks bundle deploy -t dev

# Run the job
databricks bundle run -t dev medallion_etl_job
```

## 🔄 CI/CD Workflow

### Branch Strategy

```
dev branch  →  uat branch  →  main branch
(Dev)          (UAT)          (Production)
Auto           Auto           Manual Approval
```

### Deployment Pipeline

| Branch | Environment | Job Name | Deployment |
|--------|-------------|----------|------------|
| `dev` | Development | `dev_medallion_etl_pipeline` | Automatic |
| `uat` | UAT | `uat_medallion_etl_pipeline` | Automatic |
| `main` | Production | `prod_medallion_etl_pipeline` | Manual Approval |

## 📝 Making Changes

```bash
# 1. Create feature branch
git checkout dev
git checkout -b feature/my-feature

# 2. Make changes to notebooks or config

# 3. Test locally (optional)
databricks bundle validate -t dev

# 4. Commit and push
git add .
git commit -m "feat: add new feature"
git push origin feature/my-feature

# 5. Create Pull Request → merge to dev
# → Auto-deploys to Dev

# 6. After testing, promote to UAT
git checkout uat
git merge dev
git push origin uat
# → Auto-deploys to UAT

# 7. After UAT testing, promote to Production
git checkout main
git merge uat
git push origin main
# → Requires approval → Deploys to Production
```

## 🔧 Configuration Files

### databricks.yml

Main DAB configuration defining:
- Bundle name
- Environment targets (dev, uat, prod)
- Variables and overrides
- Authentication

### resources/jobs.yml

Job definitions including:
- Task dependencies (bronze → silver → gold)
- Notebook paths
- Timeout settings
- Email notifications
- Serverless compute configuration

### .github/workflows/

GitHub Actions workflows for automated deployment:
- **deploy-dev.yml**: Deploys on push to `dev` branch
- **deploy-uat.yml**: Deploys on push to `uat` branch  
- **deploy-prod.yml**: Deploys on push to `main` branch (requires approval)

## 🧪 Testing

### Validate Bundle

```bash
databricks bundle validate -t dev
databricks bundle validate -t uat
databricks bundle validate -t prod
```

### Test Deployment

```bash
# Deploy to dev
databricks bundle deploy -t dev

# Check deployed resources
databricks bundle run -t dev --list

# Run the job
databricks bundle run -t dev medallion_etl_job
```

## 🐛 Troubleshooting

### Issue: Authentication failed

Check environment variables:
```bash
echo $DATABRICKS_HOST
echo $DATABRICKS_CLIENT_ID
echo $DATABRICKS_CLIENT_SECRET
```

### Issue: Bundle validation failed

Check YAML syntax:
```bash
databricks bundle validate -t dev
```

### Issue: Notebook not found

Verify paths in `resources/jobs.yml` match your actual notebook locations.

## 📚 Documentation

- [Databricks Asset Bundles](https://docs.databricks.com/dev-tools/bundles/)
- [GitHub Actions](https://docs.github.com/en/actions)
- [Project Documentation](../IMPLEMENTATION_GUIDE.md)

## 🤝 Contributing

1. Create feature branch from `dev`
2. Make changes
3. Test locally
4. Submit Pull Request
5. After approval, merge triggers deployment

---

**Built with ❤️ using Databricks Asset Bundles**
