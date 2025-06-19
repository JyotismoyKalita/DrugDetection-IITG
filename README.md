# Drug Detection

Creating a Logistic-Regression based Drug Detection model using features extracted from SMILES string of molecules.

This project is a part of Summer Internship at IIT Guwahati and is currenty a work in progress.

## Project Structure

```sh
.
└── 🗀 DrugDetection/
    ├── 🗀 Data/
    │   ├── 🗀 positives/
    │   │   ├── 🗀 chembl/
    │   │   │   └── 🗎 raw_approved_drug_data.csv
    │   │   └── 🗀 zinc/
    │   │       └── 🗎 word+in-man+clean.csv
    │   └── 🗀 negatives/
    │       ├── 🗀 gdb/
    │       │   └── 🗎 GDB17.50000000.smi
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
    ├── 🗎 dataset.ipynb
    └── 🗎 README.md
```

## ⚙️ Environment Setup (via Conda)

To replicate this environment, use the included environment.yml file.

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

* Python 3.10

* RDKit (chemical feature extraction)

* Pandas, NumPy (data handling)

* Scikit-learn (modeling)

* tqdm (progress bars)

* Jupyter (optional, for notebooks)

#### ❗ Troubleshooting

If RDKit fails to install: make sure you're using conda, not pip.

This environment uses the conda-forge channel for compatibility.

## Dataset

### Sources

Drugs

* ChEMBL: [https://www.ebi.ac.uk/chembl/explore/drugs/](https://www.ebi.ac.uk/chembl/explore/drugs/)

* ZINC15: [https://zinc15.docking.org/substances/subsets/world+in-man+clean/](https://zinc15.docking.org/substances/subsets/world+in-man+clean/)

Non-Drugs

* GDB17: [https://gdb.unibe.ch/downloads/](https://gdb.unibe.ch/downloads/) - Download the GDB17-Set(50 Million) and put it in "Data/negatives/gdb/". As the file is large it is not included in this repository.

* ZINC15: [https://zinc15.docking.org/tranches/home/](https://zinc15.docking.org/tranches/home/)

The dataset generating tasks were performed in ``dataset.ipynb``.

### Generating Final Dataset

**Positive(drug):**

* ChEMBL
  * Filters: Approved + Small Molecules + Not Withdrawn
  * Location: ``/Data/positives/chembl/raw_approved_drug_data.csv``

* ZINC15
  * Filters: World + In Man + Clean
  * Location: ``/Data/positives/zinc/world+in-man+clean.csv``

**Negative(non-drug):**

* GDB17 (General Chemically possible Non-Drugs)
  * Location: ``/Data/negatives/gdb/GDB17.50000000.smi``
  * To be downloaded from given link in sources.

* ZINC15
  * Filters: Clean + Lead-like
  * Location: ``/Data/negatives/zinc/*.smi``
  * "*" => CBCB, CFAC, CGBD, DCBA, DGAB, EABD, EDCA, EGAB.

#### Drug Dataset

Generating a combined Drug dataset. Total Drugs in combined Drug Dataset = **5901**

The drugs from chembl and zinc were mixed shuffled and the dataset was saved to ``Dataset/positives/dataset.csv``

#### Non-Drug Dataset

Generating a combined Non-Drug Dataset. Total non-drugs in combined Non-drug dataset = **5902**

The drugs from gdb and zinc were mixed shuffled and the dataset was saved to ``Dataset/negatives/dataset.csv``

#### Combined Dataset

Generating a Combined Dataset of both Drugs and Non-drugs. Assigned target column **"Is Drug"** with **0 for non-drugs** and **1 for drugs**.

Total Molecules = 5901(Drugs) + 5902(Non-Drugs) = 11803

The molecules of drugs and non-drugs were mixed at 1:1 ratio for better compatibility with Logistic Regression and then shuffled and the dataset was saved to ``Dataset/combined/dataset.csv``

#### Final Dataset

Preparing the final dataset with numerical features extracted from the SMILES strings.

The features extracted are:

* Physiochemical
  * Molecular Weight
  * clogP
  * TPSA
  * HBD
  * HBA
  * Rotatable Bonds
  * Ring Count
* Structural
  * EFCP4 (2048-bits)
  * MACCS (166-bits)

The final dataset was saved to ``Dataset/final/dataset.csv``

**Shape:**

* Rows: 11803 molecules (_5901 Drugs + 5902 Non-Drugs_)

* Columns: 2223 features{ 2221 descriptors (_7 physicochemical + 2048 ECFP4 + 166 MACCS_) + Smiles + Is Drug }
