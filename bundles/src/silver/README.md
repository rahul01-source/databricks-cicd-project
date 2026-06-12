# Silver Layer Notebooks

## 📝 Instructions test

Copy your silver layer notebook here:

```
02_cleanse_transactions.py
```

### How to Export from Databricks

1. Go to your workspace
2. Navigate to: `/Users/rahulmhatre173@gmail.com/End_to_End_CI_CD_Porj/silver/`
3. Right-click on `02_cleanse_transactions`
4. Select **Export** → **Source File** (`.py` format)
5. Save the file to this folder

### Alternative: Use Databricks CLI

```bash
databricks workspace export \
  /Users/rahulmhatre173@gmail.com/End_to_End_CI_CD_Porj/silver/02_cleanse_transactions \
  ./src/silver/02_cleanse_transactions.py \
  --format SOURCE
```

Once copied, this notebook will be deployed as part of the CI/CD pipeline.
