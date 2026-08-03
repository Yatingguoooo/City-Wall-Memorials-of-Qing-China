# A Computational History Study on Metadata Reconstruction and HTR Error Diagnosis Using City Wall Memorials of Qing China

This is the official repository containing the code and dataset for the graduation thesis **"A Computational History Study on Metadata Reconstruction and HTR Error Diagnosis Using City Wall Memorials of Qing China"** by **Yating Guo**.

---

## Abstract

This repository presents a dual-track digital humanities workflow applied to the **Qing Palace Memorials Database** (Taipei National Palace Museum) under the **RegInfra project**. It bridges historical metadata reconstruction with HTR model performance evaluation on Qing Dynasty city wall memorials (1736–1911).

### · Dual-Track Framework Overview

| Track | Methodology & Pipeline | Key Findings & Performance |
| :--- | :--- | :--- |
| **Track 1: Data Reconstruction** | N-gram + LLM rules + BERT pipeline cleaning 15,160 raw descriptions → **2,148 core records** | • **Reporting Monopoly**: Monopolized by governor-genral (*zongdu*) and provincial governor (*xunfu*) (Routine inspections: 36.1%).<br>• **Fiscal Shift**: State funding dropped post-1800; private donations peaked (1850–1875) during late-Qing fiscal crises.<br>• **Regional Focus**: Sichuan (Military), Shandong (Yellow River flood control), Fujian (Maritime defense). |
| **Track 2: HTR Error Diagnosis** | Page-level ($AR^*/CR^*$) diagnosis on regular vs. cursive script memorials | • **Performance Drop**: Accuracy drops from **97.47%** (Regular) to **57.87%** (Cursive).<br>• **Pathology Matrix**: Multi-character connectedness (44.91%) & shape confusion (44.18%) form the primary visual noise bottlenecks. |

### · Primary Contributions
*  **Dataset**: Provides a structured dataset of Qing city wall affairs memorials (1736–1911).
*  **Workflow**: Establishes an automated **LLM-BERT hybrid pipeline** for noisy archival metadata.
*  **Benchmark**: Builds a **cursive script error pathology matrix** to guide Human-In-The-Loop (HITL) post-processing.

---

## Repository Structure

```text
.
├── data/                         # Research & experimental datasets
│   ├── htr_benchmark/            # Handwritten Text Recognition evaluation benchmarks
│   ├── noise_terms.xlsx          # Noise terms dictionary (for N-gram rule-based filtering)
│   ├── processed.xlsx            # Preprocessed and cleaned final dataset
│   └── raw.xlsx                  # Raw, uncleaned dataset
│
├── models/                       # Trained & fine-tuned model weights
│   └── qing_citywall_model/      # BERT classification model weights for Qing city wall memorials
│
├── scripts/                      # Core pipeline scripts and notebooks
│   ├── 01_data_cleaning/         # Data cleaning & structured extraction pipeline
│   │   ├── bert_classifier/      # BERT-based text relevance classification and filtering
│   │   ├── llm_type_parser/      # LLM-based structured type parsing
│   │   └── ngram_filter/         # N-gram noise word matching & text purification
│   │
│   ├── 02_htr_evaluation/        # HTR model performance evaluation
│   │   └── pathology_analysis.ipynb # Error analysis (pathology) notebook for HTR recognition
│   │
│   └── 03_visualization/         # Data visualization & plotting
│       └── visualization.ipynb   # Notebook for generating pie charts, heatmaps, etc.
│
└── README.md                     # Project documentation

```

## Environment Requirements

* **Python Version**: `3.13.5`
* **Core Dependencies**: `pandas`, `torch`, `transformers`, `seaborn`, `matplotlib`, `numpy`, `tqdm`.

---

## Data Retrieval Protocol

The raw dataset was directly harvested from the official database using exact Boolean query logic (e.g., combining wall-related keywords via `AND` operators). Consequently, all 15,160 retrieved catalog items inherently contained high-relevance terms regarding city wall engineering, removing the need for a secondary dictionary-based topic filter.

---

## Data Dictionary & Codebook Explanation (`processed.xlsx`)

To reconcile modern spatial mapping with historical geopolitical realities, the author designed a dual-level spatial encoding scheme. While fields `P1`–`P3` represent standard modern provincial-level entities for macroscopic visualization, fields `P1_1`–`P3_1` account for Qing-specific administrative and military jurisdictions (e.g., mapping Ili directly as `P1_1 = Ili` under the Xinjiang/Gansu regional umbrella). This adaptation ensures historical accuracy without compromising modern spatial comparability.

| Column Field | Description | Details & Examples |
| :--- | :--- | :--- |
| `P1`, `P2`, `P3` | Standard Provincial Boundaries | Used for macro-spatial statistics and geographic mapping (e.g., integrating Ili into Xinjiang or Gansu). |
| `P1_1`, `P2_1`, `P3_1` | Historical Administrative Adaptation (Qing dynasty) | Faithfully preserves Qing-specific military governorates (*Jiangjun*) or special regions. E.g., when `P1 = Xinjiang/Gansu`, the historical entity is specified as `P1_1 = Ili` (*Ili General Region*). |
| `P_patch` | Administrative Patching & Outlier Correction | Used to patch missing provincial attributes based on specific city names or spatial context (e.g., filling in Shandong or Zhili). |
| `OP` | Official Title of Memorialist | The full official title of the official submitting the memorial. |
| `Core_OP` | Core Official Post | The primary or most significant official title extracted from `OP`. |
| `Type` (in `all provinces` sheet) | AI-Predicted Memorial Types | Automated type classification generated by trained models. |
| `Type` (in `type` sheet) | Human-Verified Memorial Types | Manually verified and curated memorial categories. **Note:** A single `Abstract` entry may correspond to multiple `Type` labels (one-to-many relationship). |

---

## Demo Files & Pipeline Batch Processing

Due to the large volume of historical archival data, the author executed data processing and prediction in **batches**. The outputs of these batches were subsequently consolidated manually into the master spreadsheet (`processed.xlsx`). 

To facilitate quick testing and reproduction, demo files are provided in each submodule under `scripts/01_data_cleaning/`:

* **`bert_classifier/`**: Contains `bert_classifier.ipynb` (execution code), alongside `correlation_demo_input.xlsx` and `correlation_demo_output.xlsx` for end-to-end testing.
* **`llm_type_parser/`** & **`ngram_filter/`**: Structured identically with corresponding scripts and small demo input/output files.

---

## HTR Evaluation Setup (`pathology_analysis.ipynb`)

To run the HTR pathology analysis notebook (`scripts/02_htr_evaluation/pathology_analysis.ipynb`):

>  **Note on Directory Placement**: The `pathology_analysis.ipynb` script must be placed directly inside the specific memorial subfolder under `data/htr_benchmark/` before execution.
> 
> **Example Path**:  
> `data/htr_benchmark/楷_daoguang_1842-10-27_fujian_xx_xx_gugong112812/pathology_analysis.ipynb`