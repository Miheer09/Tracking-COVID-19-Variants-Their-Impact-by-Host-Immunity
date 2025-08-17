# 🧬 COVID-19 Variant Evolution & Risk Analysis

**A data-driven exploration of SARS-CoV-2 variants**  
Tracking how mutations evolved and how variant waves relate to clinical severity across different host immunity profiles.

---

## 📌 Overview
This project integrates **genomic analysis** (mutation matrix + PCA) with **synthetic clinical metadata** (age, sex, vaccination, comorbidities, severity).  
It produces:
- Mutation PCA plots by lineage
- Variant timelines (monthly proportions) with **severity overlays**
- A logistic regression risk model for severe outcomes
- Risk matrices (Variant × Vaccination)

> :exclamation: Clinical data in this repository is **synthetic** and for demonstration only.

---

## 📂 Repository Structure
```plaintext
├── data/
│   ├── sequences.fasta                      # SARS-CoV-2 sequences (FASTA)
│   ├── India_metadata.csv                   # Genomic metadata
│   ├── synthetic_clinical_data.csv          # Synthetic clinical dataset (included)
│   └── metadata_with_synthetic_clinical.csv # Merged table used by notebook (generated)
├── figures/                                 # Plots saved by the notebook (generated)
├── outputs/                                 # CSV outputs
├── project_analysis.ipynb                   # The ONLY notebook you run
└── README.md
```


---

## :inbox_tray: Datasets
- **Genomic sequences & metadata (source):** NCBI Virus — https://www.ncbi.nlm.nih.gov/labs/virus/  
  (You can filter by organism “SARS-CoV-2” and location “India”, then download FASTA + metadata CSV.)
- **Synthetic clinical dataset (included):** `data/synthetic_clinical_data.csv`  
  Generated in this repo to enable variant × host severity analysis.

> Optional: If you have GISAID access, you can substitute GISAID sequences/metadata.

---

## :microscope: Synthetic Clinical Data: How We Built It
We generated realistic fields to mirror expected distributions and known risk gradients:
- **host_age:** skewed toward adults; fewer pediatric and very-elderly counts.
- **host_sex:** ≈52% Male, 48% Female.
- **vaccination_status:** `Unvaccinated`, `Partially vaccinated`, `Fully vaccinated`, `Boosted` (optional).
- **prior_infection:** small baseline probability; slightly higher among vaccinated groups.
- **comorbidities:** `None`, `Diabetes`, `Hypertension`, `Multiple` with **age-dependent** probabilities:  
  - `<30`: mostly `None`; rare diabetes/hypertension.  
  - `30–50`: higher `Hypertension`, moderate `Diabetes`.  
  - `>50`: increased `Multiple` comorbidities.
- **disease_severity:** `Mild`, `Moderate`, `Severe`, `Death` derived from a risk score combining:  
  age (↑ risk with age), comorbidities (`Multiple` > `Hypertension`/`Diabetes` > `None`), vaccination (protective), and variant category (Delta > Other > Omicron).

These rules are **illustrative**; adjust probabilities in the notebook to match your study assumptions.

---

## 🚀 How to Run (Single Notebook)
1. **Install dependencies**
   ```bash
   pip install pandas numpy biopython matplotlib seaborn scikit-learn
   ```
2. **Open the notebook**
   - `project_analysis.ipynb`
3. **Edit paths** at the top (if needed) to point to your `data/` files.
4. **Run all cells** to:
   - Compute mutation matrix & PCA  
   - Generate synthetic clinical data & merge with metadata  
   - Train logistic regression and plot performance  
   - Produce variant timelines with severity overlays  
   - Export figures to `figures/` and CSVs to `outputs/`

---

## 📈 Example Outputs
- **PCA of Mutation Profiles:** clusters by Pangolin lineage  
- **AUC ≈ 0.81** on test for severe outcome classifier (example run)  
- **Risk Factors (top):** Unvaccinated, Age, Multiple comorbidities (example)  
- **Variant Timeline:** stacked area of monthly proportions + severity % overlay

> Your exact numbers will vary depending on filters, time windows, and seeds.

---

## :repeat_one: Reproducibility
- We set random seeds for NumPy/Scikit-Learn in the notebook.
- All generated artifacts (merged CSVs, figures) are saved to `outputs/` and `figures/` for traceability.

---

## ⚠️ Disclaimer
- The clinical dataset is **synthetic** and **not** derived from real patients.  
- Do **not** use these outputs for medical or public-health decisions.

---


## 🙌 Acknowledgements
- NCBI Virus for public viral sequence resources.
- Pangolin for lineage annotations (as referenced in metadata).

