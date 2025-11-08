# MLOps DVC Demo — Data Version Control Hands-On  

## 🚀 Overview  
This repository demonstrates **Data Version Control (DVC)** as part of an MLOps workflow.  
It walks through setting up Git, connecting remote storage, versioning data, and tracking changes in datasets via DVC commands.  

You’ll also learn how to version your dataset (`data/` folder) using **Git + DVC** and reproduce older dataset versions easily.  

---

## ⚙️ Prerequisites  
- Python ≥ 3.8  
- Git installed and configured  
- AWS S3 bucket (for remote storage)  
- DVC installed (`pip install dvc`)  

---

## 📂 Repository Structure  
```

mlops_dvc_4/
├─ data/              # your dataset folder
│  └─ sample.csv      # sample data file for demo
├─ src/
│  ├─ mycode.py       # demo script that modifies or appends data
│  └─ ...
├─ .dvc/              # DVC internal folder (created after dvc init)
├─ .dvcignore
├─ data.dvc           # metadata file created after dvc add
├─ requirements.txt
└─ README.md

````

---

## 🧩 Step-by-Step Implementation (Follow in Sequence)

### 🥇 Step 1 — Create & initialize a Git repository  
```bash
# create repo on GitHub (example: mlops_dvc_4)
# then clone it locally
git clone https://github.com/varunchach/mlops_dvc_4.git
cd mlops_dvc_4
````

---

### 🥈 Step 2 — Set up virtual environment

```bash
python -m venv venv1
.\venv1\Scripts\activate     # on Windows
# source venv1/bin/activate  # on Linux / Mac
```

---

### 🥉 Step 3 — Add a sample CSV inside the `data/` folder

Create a folder named `data` in your project directory and add the following CSV file as `sample.csv` inside it.

```csv
name,age,city
Varun,28,Mumbai
Ravi,32,Pune
Neha,25,Delhi
```

You can later modify this file to simulate data changes during DVC demo.

---

### 🏁 Step 4 — Add & commit files before enabling DVC

```bash
git init
git add .
git commit -m "Commit before DVC"
git remote remove origin
git remote add origin https://github.com/varunchach/mlops_dvc_1.git
git push origin main
```

---

### 🧱 Step 5 — Install DVC

```bash
pip install dvc
```

---

### 🧩 Step 6 — Initialize DVC in the repository

```bash
dvc init
```

This creates `.dvc/` and `.dvcignore` folders.

---

### 🌐 Step 7 — Add remote storage

```bash
dvc remote add -d myremote S3
```

> Replace `S3` with your actual S3 path, e.g. `s3://mybucket/dvcstore`

---

### 🗂️ Step 8 — Track data using DVC

Initially, `data/` is tracked by Git. We need to untrack it and let DVC handle it.

```bash
dvc add data/
# if prompted:
git rm -r --cached "data"
git commit -m "Stop tracking data in Git"
```

Now add again to ensure DVC tracking:

```bash
dvc add data/
git add .gitignore data.dvc
dvc commit
dvc push
```

Mark this as your **first data version**:

```bash
git add .
git commit -m "Version 1 - data tracked by DVC"
git push origin main
```

---

### 🧮 Step 9 — Modify code and create a new data version

Edit `src/mycode.py` to append or modify a row in your dataset.

```python
# src/mycode.py
import pandas as pd

df = pd.read_csv("data/sample.csv")
df.loc[len(df)] = ["Asha", 29, "Chennai"]
df.to_csv("data/sample.csv", index=False)
print("✅ Added new row to dataset!")
```

Then check DVC status:

```bash
dvc status
```

Commit and push the new version:

```bash
dvc commit
dvc push
git add .
git commit -m "Version 2 - updated data"
git push origin main
```

---

### 🧾 Step 10 — Verify versions and sync

Check everything is up to date:

```bash
git status
dvc status
```

View Git history:

```bash
git log --oneline
```

Restore an older version:

```bash
git checkout <commit_id>
dvc pull
```

---

## 🧠 Concept Recap

| Step | Command                         | Purpose                          |
| ---- | ------------------------------- | -------------------------------- |
| 1    | `dvc init`                      | Initializes DVC project          |
| 2    | `dvc add data/`                 | Tracks dataset with DVC          |
| 3    | `dvc remote add -d myremote S3` | Connects remote storage          |
| 4    | `dvc commit`, `dvc push`        | Saves and uploads versioned data |
| 5    | `dvc pull`                      | Retrieves older versions         |
| 6    | `git add/commit/push`           | Tracks code & DVC metadata       |

---

## ✅ Final Checks

* `git status` → should be clean
* `dvc status` → “Data and pipelines are up to date.”
* `dvc list` → lists tracked artifacts
* `git log --oneline` → see commit history

---

## 🧾 Summary

This project illustrates how **DVC** complements **Git** to handle large datasets and maintain version control for data and models.
The sample CSV provided ensures learners can immediately practice and visualize DVC data tracking and versioning in action.

```
```
