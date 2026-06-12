# Gold Layer Notebooks

## 📝 Instructions

Copy your gold layer notebook here:

```
03_business_metrics.py
```

### How to Export from Databricks

1. Go to your workspace
2. Navigate to: `/Users/rahulmhatre173@gmail.com/End_to_End_CI_CD_Porj/gold/`
3. Right-click on `03_business_metrics`
4. Select **Export** → **Source File** (`.py` format)
5. Save the file to this folder

### Alternative: Use Databricks CLI

```bash
databricks workspace export \
  /Users/rahulmhatre173@gmail.com/End_to_End_CI_CD_Porj/gold/03_business_metrics \
  ./src/gold/03_business_metrics.py \
  --format SOURCE
```

Once copied, this notebook will be deployed as part of the CI/CD pipeline.
