# 💸 Lending Club Loan Data — Comprehensive Exploratory Data Analysis

> Reproducible EDA pipeline for the Lending Club `accepted_2007_to_2018Q4.csv` dataset (2007–2018).

---

## 🔎 Purpose

This repository contains a fully documented Jupyter notebook and supporting scripts that perform a complete exploratory data analysis (EDA) on the Lending Club loan dataset. The README below gives **clear, step-by-step instructions** to **set up the environment**, **install dependencies** (via `requirements.txt`), and **run the analysis** so you — or anyone — can reproduce the findings exactly.

---



# ✅ Quick install & reproduce (step-by-step)

Follow this section exactly to reproduce the results from scratch.

> These steps assume you have Python 3.8–3.10 installed. If you prefer conda or Docker, see the alternative instructions below.

### 1) Clone repository

```bash
git clone <repository-url>
cd lending-club-eda
```

### 2) Download dataset

* Download `accepted_2007_to_2018Q4.csv` from the Lending Club **Statistics** page (Lending Club site) and place it in `data/`.
* If you prefer a path elsewhere, update the variable `DATA_PATH` at the top of `lending_club_eda.ipynb`.

### 3) Create & activate a virtual environment (venv)

```bash
python -m venv lending_env
# macOS / Linux
source lending_env/bin/activate
# Windows (PowerShell)
# .\lending_env\Scripts\Activate.ps1
```

### 4) Install dependencies using `requirements.txt`

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

**Minimal `requirements.txt`** (the repository includes a pinned file — example below):

```
pandas==2.2.2
numpy==1.26.2
matplotlib==3.8.1
seaborn==0.12.2
jupyter==1.0.0
ydata-profiling==4.3.1
phik==0.12.3
scipy==1.11.1
statsmodels==0.14.0
numba==0.57.0
openpyxl==3.1.2
```

> Tip: Use the pinned `requirements.txt` in the repo to avoid version mismatch.

### 5) (Optional) Create Conda environment

If you prefer conda, use the included `environment.yml` (if present) or create one like:

```yaml
name: lending-eda
channels:
  - conda-forge
dependencies:
  - python=3.9
  - pandas
  - numpy
  - matplotlib
  - seaborn
  - jupyter
  - pip
  - pip:
    - ydata-profiling
    - phik
```

Create & activate:

```bash
conda env create -f environment.yml
conda activate lending-eda
```

### 6) Run the notebook interactively (recommended)

```bash
jupyter notebook
# then open lending_club_eda.ipynb and run cells top-to-bottom
```

### 7) Run the notebook non-interactively (reproduce exact outputs)

The repository contains `run_analysis.sh` (executable) which runs the notebook headlessly and writes artifacts to `generated_reports/`.

Make it executable and run:

```bash
chmod +x run_analysis.sh
./run_analysis.sh
```

Or run directly with `nbconvert`:

```bash
jupyter nbconvert --execute --inplace --ExecutePreprocessor.timeout=600 lending_club_eda.ipynb
```

This will:

* generate `generated_reports/lending_club_eda_report.html`
* export `generated_reports/missing_values_report.csv`
* create PNG visualizations under `generated_reports/visualization_plots/`

---

## 🔁 Reproducibility tips (ensure identical results)

* The notebook uses `RANDOM_STATE = 42` (or similar) for sampling and train/test splits — keep this unchanged to reproduce numbers/plots exactly.
* If you sample the dataset for speed, note the sample size in the notebook header (e.g. `SAMPLE_SIZE = 100000`). Using a different sample size will change statistics.
* Save the generated artifacts with timestamps if you run multiple experiments to avoid overwriting earlier results.

---



## ⚙️ Configuration flags inside the notebook

At the top of `lending_club_eda.ipynb` you'll find a configuration cell. Edit these variables to change run behavior:

```python
DATA_PATH = 'data/accepted_2007_to_2018Q4.csv'
GENERATE_REPORT = True       # toggle ydata-profiling generation
SAMPLE_SIZE = 100000        # set to None to use full dataset
MINIMAL_PROFILE = True      # minimal=True for ydata-profiling to speed up
RANDOM_STATE = 42
OUTPUT_DIR = 'generated_reports'
```

---

## 🔬 What is produced (expected artifacts)

After a successful run you should find:

* `generated_reports/lending_club_eda_report.html` — interactive ydata profiling report
* `generated_reports/missing_values_report.csv` — per-column missing percentage and counts
* `generated_reports/visualization_plots/*.png` — static plots used in the notebook
* Console logs in the terminal showing processing steps (data load → profiling → exports)

Exact filenames are created by the notebook — check `OUTPUT_DIR` variable if you changed it.

---

## 🛠 Troubleshooting (common problems)

* **MemoryError / notebook crashes**

  * Use sampling (`SAMPLE_SIZE = 50000`) or run on a machine with more RAM or cloud (Colab/Kaggle/AWS).
  * Set `MINIMAL_PROFILE = True` to reduce profiling memory.

* **ydata-profiling fails to install or run**

  * Try pinning to the version listed in `requirements.txt`.
  * Run `pip install ydata-profiling==4.3.1` and restart kernel.

* **FileNotFoundError**

  * Ensure `data/accepted_2007_to_2018Q4.csv` exists and `DATA_PATH` correctly points to it.

* **Slow plotting / long runtime**

  * Reduce `SAMPLE_SIZE`.
  * Turn off the automated profiling and run only essential plotting cells.

---

## 🔎 Example `run_analysis.sh` (included in repo)

```bash
#!/usr/bin/env bash
set -e

# Activate venv if script is run from a shell where venv exists
if [ -d "lending_env" ]; then
  source lending_env/bin/activate
fi

jupyter nbconvert --execute --inplace --ExecutePreprocessor.timeout=1200 lending_club_eda.ipynb

# Move or list generated reports
ls -lh generated_reports || true
```

---

## 📌 Good practices for sharing results

* Do **not** commit large dataset files to GitHub. Add `data/accepted_2007_to_2018Q4.csv` to `.gitignore` if you run locally.
* Keep `generated_reports/` in `.gitignore` unless the produced HTML is small and intended for the repo. Otherwise, publish reports as GitHub Releases or store them externally (e.g., S3).
* Include `requirements.txt` and `environment.yml` in the repo root for easy setup.

---

## 🧾 Licensing

This project is licensed under the **MIT License**. See `LICENSE` for full terms.

---

## ❤️ Contributions

Contributions are welcome. Please open issues describing bugs or feature requests and use feature branches for PRs. Suggested branch naming: `feature/<short-desc>`.

---

## 📞 Support

If the notebook errors, open an issue and include:

* The exact error traceback
* Python version (`python --version`)
* OS
* contents of `requirements.txt`

---


