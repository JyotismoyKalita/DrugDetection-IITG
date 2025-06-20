# 💊 Drug Detection

## 📘 Overview

Creating a Logistic-Regression based Drug Detection model using features extracted from SMILES string of molecules.

This project is a group effort and part of Summer Internship at IIT Guwahati and is currenty a work in progress.

### 👥 Team Members

- **Anwesha Chaudhury**  
  [GitHub](https://github.com/chan1425) | [LinkedIn](https://www.linkedin.com/in/anwesha-chaudhury-67a050292/)

- **Jyotismoy Kalita**  
  [GitHub](https://github.com/JyotismoyKalita/) | [LinkedIn](https://www.linkedin.com/in/jyotismoy-kalita/)

- **Sampurna Biswas**  
  [GitHub](https://github.com/SamPurna023) | [LinkedIn](https://www.linkedin.com/in/sampurna-biswas-7487bb290/)

## 🗂️ Project Structure

```sh
.
└── 🗀 DrugDetection/
    ├── 🗀 Data/
    │   ├── 🗀 positives/
    │   │   ├── 🗀 chembl/
    │   │   │   └── 🗎 raw_approved_drug_data.csv
    │   │   └── 🗀 zinc/
    │   │       └── 🗎 world+in-man+clean.csv
    │   └── 🗀 negatives/
    │       ├── 🗀 gdb/
    │       │   ├── 🗀 CN/
    │       │   │   ├── 🗎 1.cn.smi
    │       │   │   •
    │       │   │   •
    │       │   │   •
    │       │   │   └── 🗎 9.cn.smi
    │       │   ├── 🗀 CO/
    │       │   │   ├── 🗎 1.co.smi
    │       │   │   •
    │       │   │   •
    │       │   │   •
    │       │   │   └── 🗎 9.co.smi
    │       │   └── 🗀 SaturatedHydrocarbons/
    │       │       ├── 🗎 1.g.smi
    │       │       •
    │       │       •
    │       │       •
    │       │       └── 🗎 11.g.smi
    │       └── 🗀 zinc/
    │           ├── 🗎 CBCB.smi
    │           ├── 🗎 CFAC.smi
    │           ├── 🗎 CGBD.smi
    │           ├── 🗎 DCBA.smi
    │           ├── 🗎 DGAB.smi
    │           ├── 🗎 EABD.smi
    │           ├── 🗎 EDCA.smi
    │           └── 🗎 EGAB.smi
    ├── 🗀 Dataset/
    │   ├── 🗀 positives/
    │   │   └── 🗎 dataset.csv
    │   ├── 🗀 negatives/
    │   │   └── 🗎 dataset.csv
    │   ├── 🗀 combined/
    │   │   └── 🗎 dataset.csv
    │   └── 🗀 final/
    │       └── 🗎 dataset.csv
    ├── 🗀 Model/
    │   ├── 🗎 final_model.pkl
    │   ├── 🗎 scaler.pkl
    │   ├── 🗎 selectkbest.pkl
    │   └── 🗎 variance_thresold.pkl
    ├── 🗎 .gitignore
    ├── 🗎 environment.yaml
    ├── 🗎 dataset.ipynb
    ├── 🗎 model.ipynb
    └── 🗎 README.md
```

## ⚙️ Environment Setup (via Conda)

To replicate this environment, use the included environment.yml file. The environment.yml file can be used to replicate the Python environment via Conda.

✅ Step-by-step:
Clone the repository:

```bash
git clone https://github.com/YourUsername/DrugDetection-IITG.git
cd DrugDetection-IITG
```

Create the conda environment:

```bash
conda env create -f environment.yml
```

Activate the environment:

```bash
conda activate drug-detection
```

(Optional) If using Jupyter Notebooks:

```bash
jupyter notebook
```

### 📦 Included in the Environment

- Python 3.10

- RDKit (chemical feature extraction)

- Pandas, NumPy (data handling)

- Scikit-learn (modeling)

- tqdm (progress bars)

- Jupyter (optional, for notebooks)

#### ❗ Troubleshooting

If RDKit fails to install: make sure you're using conda, not pip.

This environment uses the conda-forge channel for compatibility.

## 📦 Data Preperation

### 🔗 Sources

Drugs

- ChEMBL: [https://www.ebi.ac.uk/chembl/explore/drugs/](https://www.ebi.ac.uk/chembl/explore/drugs/)

- ZINC15: [https://zinc15.docking.org/substances/subsets/world+in-man+clean/](https://zinc15.docking.org/substances/subsets/world+in-man+clean/)

Non-Drugs

- GDB: [https://gdb.unibe.ch/downloads/](https://gdb.unibe.ch/downloads/)

- ZINC15: [https://zinc15.docking.org/tranches/home/](https://zinc15.docking.org/tranches/home/)

The dataset generating tasks were performed in ``dataset.ipynb``.

 ---

### Generating Dataset

**Positive(drug):**

- ChEMBL
  - Filters: Approved + Small Molecules + Not Withdrawn
  - Location: ``/Data/positives/chembl/raw_approved_drug_data.csv``

- ZINC15
  - Filters: World + In Man + Clean
  - Location: ``/Data/positives/zinc/world+in-man+clean.csv``

**Negative(non-drug):**

- GDB
  - Subsets: gdb13.g.tgz, gdb13.co.tgz, gdb13.cn.tgz
  - Location: ``/Data/negatives/gdb/``
    - ``CN/``: unpacked gdb13.cn.tgz
    - ``CO/``: unpacked gdb13.co.tgz
    - ``SaturatedHydrocarbons/``: unpacked gdb13.g.tgz

- ZINC15
  - Filters: Clean + Lead-like
  - Location: ``/Data/negatives/zinc/*.smi``
  - "*" => CBCB, CFAC, CGBD, DCBA, DGAB, EABD, EDCA, EGAB.

---

#### ✅ Drug Dataset

Generating a combined Drug dataset. Total Drugs in combined Drug Dataset = **5901**

The drugs from ChEMBL and ZINC15 were combined, shuffled, and saved to `Dataset/positives/dataset.csv`.

---

#### ❌ Non-Drug Dataset

Generating a combined Non-Drug Dataset. Total non-drugs in combined Non-drug dataset = **5902**

The non-drugs from GDB and ZINC15 were combined, shuffled, and saved to `Dataset/negatives/dataset.csv`.

---

#### 🔀 Combined Dataset

Generating a Combined Dataset of both Drugs and Non-drugs. Assigned target column **"Is Drug"** with **0 for non-drugs** and **1 for drugs**.

Total Molecules = 5901(Drugs) + 5902(Non-Drugs) = 11803

The molecules of drugs and non-drugs were mixed at 1:1 ratio for better compatibility with Logistic Regression and then shuffled and the dataset was saved to file ``Dataset/combined/dataset.csv``

---

#### 🧮 Final Dataset

Preparing the final dataset with numerical features extracted from the SMILES strings.

The features extracted are:

- Physicochemical
  - Molecular Weight
  - clogP
  - TPSA
  - HBD
  - HBA
  - Rotatable Bonds
  - Ring Count
- Structural
  - ECFP4 (2048-bits)
  - MACCS (166-bits)

The final dataset was saved to ``Dataset/final/dataset.csv``

**📏 Shape:**

- Rows: 11803 molecules (_5901 Drugs + 5902 Non-Drugs_)

- Columns: 2223 features{ 2221 descriptors (_7 physicochemical + 2048 ECFP4 + 166 MACCS_) + Smiles + Is Drug }

## 🎛️ Preprocessing and Model Training & Evaluation

## 1️⃣ Data Preprocessing

- Removed:
  - Duplicate `SMILES`-based rows
  - Full row duplicates
  - Rows with `NaN` or infinite values
- Split:
  - `X` (features) = all except `smiles` and `Is Drug`
  - `y` = `Is Drug` column

---

## 2️⃣ Train-Test Split

- 80% training, 20% testing using `train_test_split`
- Stratified by target class

---

## 3️⃣ Feature Exploration

- Visualized 7 descriptors using:
  - 📦 Boxplots
  - 📊 Histograms with KDE

---

## 4️⃣ Feature Transformation

- Applied **log1p()** to reduce skewness for:
  - `MW`, `TPSA`, `HBD`, `HBA`, `Rotatable Bonds`
- Used **RobustScaler** to scale all 7 descriptors

---

## 5️⃣ Feature Selection

### 🔹 A. Low-Variance Filtering

- Removed fingerprint features with variance < 0.01 using `VarianceThreshold`

### 🔹 B. Feature Combination

- Concatenated:
  - Scaled descriptors
  - Reduced fingerprint vectors

### 🔹 C. Top-K Feature Selection

- Selected **top 300** features via `SelectKBest` using ANOVA F-test (`f_classif`)

---

## 6️⃣ Cross-Validation

- 5-fold cross-validation on training set
- Scoring metric: `roc_auc`
- Reported:
  - Individual AUC scores
  - Mean ± Standard Deviation

---

## 7️⃣ Final Model Training & Evaluation

- Refit Logistic Regression on **entire training set**
- Evaluated on **test set**:
  - Accuracy
  - ROC-AUC
  - Precision, Recall, F1-score
  - Confusion Matrix (Raw + Normalized)
- Replotted ROC curve and confusion matrix

---

## 8️⃣ Model and Preprocessing objects Export

- The final trained Logistic Regression model and the scaler, selector, variance thresold object was saved using joblib for future use.
- This model can later be loaded via:

  ```python
  from joblib import load

  scaler = load("scaler.pkl")
  vt = load("variance_threshold.pkl")
  selector = load("selectkbest.pkl")
  model = load("final_model.pkl")
  ```

## ✅ Final Outcome

- End-to-end drug detection pipeline
- Model performs well and generalizes across folds
- Clear visualizations and evaluation metrics
- Pipeline is modular and reproducible, and includes saved model, selector, scaler for deployment

## 📊 Final Evaluated Results on Test Set

- **ROC-AUC Score**: `0.9721`

- **Classification Report**:

  ```yaml
  ROC-AUC Score: 0.9721

  Classification Report:

                  precision    recall  f1-score   support

             0      0.91      0.91      0.91      1181
             1      0.91      0.91      0.91      1180

      accuracy                          0.91      2361
    macro  avg      0.91      0.91      0.91      2361
  weighted avg      0.91      0.91      0.91      2361

  ```
