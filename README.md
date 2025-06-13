<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4d/Hebrew_University_Logo.svg/1200px-Hebrew_University_Logo.svg.png" alt="huji-logo" height="180">
</p>

<div align="center">

# IML Hackathon 2025 – Predicting Breast-Cancer Attributes  
<sub>Course ❯ <strong>Introduction to Machine Learning (67577)</strong> • School of Computer Science & Engineering, Hebrew University of Jerusalem</sub>

</div>



## Table of Contents  
1. [Project Brief](#project-brief)  
2. [Repository Structure](#repository-structure)  
3. [Installation & Quick-Start](#installation--quick-start)  
4. [Part 1 - Metastasis Prediction](#part-1--metastasis-prediction)  
5. [Part 2 - Tumor-Size Regression](#part-2--tumor-size-regression)  
6. [Part 3 - Unsupervised Insights](#part-3--unsupervised-insights)  
7. [Reproducibility Checklist](#reproducibility-checklist)  
8. [Authors & Credits](#authors--credits)  
9. [License](#license)



## Project Brief
Breast-cancer follow-up often involves dozens of clinical features measured across repeated patient visits.  
During **Hackathon 2025** we tackled three independent mini-challenges on an anonymised national cohort:

| Task | Goal | Metric | Entry-point |
|------|------|--------|-------------|
| **Part 1** | *Multi-label* classification of future metastasis sites (0–3 labels/visit) | Micro & Macro **F<sub>1</sub>** | `subtaskI/part1.py` |
| **Part 2** | Regression of diagnosed tumour size (mm) | Mean-Squared-Error | `subtaskII/part2.py` |
| **Part 3** | Purely *unsupervised* exploration: PCA, t-SNE, K-Means | Qualitative | `subtaskIII/unsupervised_learning.py` |

The dataset (≈ 50 k visits, 34 raw columns) was provided by the course staff.



## Repository Structure
```text
.
├── data/                         ←  CSVs delivered by the organisers (not tracked)
│   └── train_test_splits/
├── src/
│   ├── load_and_split.py         # IO + stratified splitting helpers
│   ├── process_data.py           # Feature cleaning & engineering
│   ├── labels_utils.py           # Multi-hot encoder / decoder
│   └── models.py                 # Factory for all sklearn models
├── subtaskI/
│   ├── part1.py                  # Train → dev evaluation → test prediction
│   ├── predictions.csv           # Submission file
│   └── requirements.txt
├── subtaskII/
│   ├── part2.py
│   ├── predictions.csv
│   └── requirements.txt
├── subtaskIII/
│   ├── unsupervised_learning.py
│   └── project.pdf               # Illustrated report
├── README.md
├── requirements.txt              # Dev-time deps
└── LICENSE
```

## Installation & Quick-Start
### 1. Clone and create a clean virtual-env
`git clone https://github.com/ShayMorad/Hackathon-IML.git ` \
`cd Hackathon-IML ` \
`python -m venv .venv && source .venv/bin/activate` 

### 2. Install core dependencies
`pip install -r requirements.txt`         
>numpy, pandas, scikit-learn, matplotlib …

### 3. Drop the provided CSVs under ./data/train_test_splits 
 >Data removed for patients privacy, can use your own data with out code & models.

### 4. Run Part 1 end-to-end
`python subtaskI/part1.py 
   ./data/train_test_splits/train.feats.csv 
   ./data/train_test_splits/train.labels.0.csv 
   ./data/train_test_splits/test.feats.csv`

### 5. Run the official scorer on the dev split
`python src/evaluate_part_0.py 
   --gold ./data/train_test_splits/train.labels.0.csv 
   --pred subtaskI/predictions.csv`

## Part 1 – Metastasis Prediction <a name="part-1--metastasis-prediction"></a>

**Pipeline**

| Stage | Notes |
|-------|-------|
| Pre-processing | 50 + heuristics to normalise noisy categorical strings (ER/PR/HER2 status, surgery codes, …); engineered 38 numeric & one-hot features. |
| Candidates | Logistic Regression · KNN · Decision Tree · **Random Forest** · Ridge. |
| Best model | Random Forest (200 trees, class-balanced) → dev **micro F₁ ≈ 0.35**. |


## Part 2 – Tumor-Size Regression <a name="part-2--tumor-size-regression"></a>

* **Target range:** 0 – 140 mm (negatives clipped to 0).  
* **Models tried:** Linear, ElasticNet, SGD, **Gradient Boosting Trees**.  
* **Winner:** GBT with dev **MSE ≈ 1.2**.  
* **Insights:** Top features – TNM-T stage, Ki-67 percentage, histological grade.



## Part 3 – Unsupervised Insights <a name="part-3--unsupervised-insights"></a>


| Technique | Highlight |
|-----------|-----------|
| **PCA (10 compon.)** | PC1 (≈ 21 % variance) correlates with tumour burden & surgery count. |
| **K-Means (k = 5)** | One cluster enriched for *young patients + high Ki-67 + multiple surgeries* → potential high-risk phenotype. |
| **t-SNE** | Confirms separability; lobular carcinomas occupy a distinct sub-manifold. |

> Full narrative, scree plots & silhouette analysis in `subtaskIII/project.pdf`.


## Reproducibility Checklist <a name="reproducibility-checklist"></a>

- Python 3.11 
- Fixed random seeds (`numpy`, `sklearn`).  
- No test leakage – test CSVs only loaded post-training.  



## Authors & Credits <a name="authors--credits"></a>

| Name       |
|------------|
| **Shay M.** |
| **Lior T.** |
| **Yehonatan E.** |
| **Abraham K.** |

*Supervision:* Dr. Gabriel Stanovsky et al. – course staff & dataset providers.



## License <a name="license"></a>
This project is released under the **MIT License** (see `LICENSE`).  
> **Disclaimer:** Patient data were fully anonymised; models are for research use only.
