# 🔍 Hallucination Detection in Large Language Models Using Machine Learning

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research%20Paper-orange)
![Institution](https://img.shields.io/badge/Institution-VIT%20Chennai-purple)

> A comparative study of classical and ensemble machine learning techniques for detecting hallucinated responses in Large Language Model outputs, using the HaluEval benchmark dataset.

---

## 📌 Overview

Large Language Models (LLMs) frequently generate **hallucinated content** — responses that are fluent and confident but factually incorrect or unsupported by any reference source. This project frames hallucination detection as a **supervised binary classification problem**:

- **Label 0** → Factually Consistent answer  
- **Label 1** → Hallucinated answer

Five machine learning classifiers are trained on **TF-IDF vectorized** answer text and evaluated on a held-out test set. The study establishes a lightweight, reproducible baseline that achieves over **92% accuracy** without requiring transformer fine-tuning.

---

## 🏆 Results at a Glance

| Model | Accuracy | AUC |
|---|---|---|
| Logistic Regression | 90.38% | 0.956 |
| CatBoost | 92.00% | 0.960 |
| LightGBM | 92.25% | 0.950 |
| Random Forest | 92.50% | **0.967** |
| **XGBoost** ⭐ | **92.75%** | 0.956 |

> **Key finding:** XGBoost achieves the highest accuracy, but Random Forest leads on AUC — indicating its predicted probabilities are better calibrated across all classification thresholds.

---

## 📁 Repository Structure

```
hallucination-detection/
│
├── 📓 notebooks/
│   └── hallucination_detection.ipynb   # Full pipeline: preprocessing → training → evaluation
│
├── 📊 results/
│   ├── results_export.json             # Model predictions, probabilities, confusion matrices
│   └── README.md                       # Description of results data structure
│
├── 📄 paper/
│   └── README.md                       # Paper details and authors
│
├── 📂 data/
│   └── README.md                       # How to obtain the HaluEval dataset
│
├── requirements.txt                    # Python dependencies
├── .gitignore                          # Files excluded from version control
└── README.md                           # This file
```

---

## ⚙️ Setup and Installation

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/hallucination-detection.git
cd hallucination-detection
```

### 2. Create a virtual environment (recommended)
```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Get the dataset
The HaluEval dataset is available on Kaggle and Hugging Face. See [`data/README.md`](data/README.md) for download instructions.

### 5. Run the notebook
```bash
jupyter notebook notebooks/hallucination_detection.ipynb
```

---

## 🔬 Methodology

The pipeline follows these steps:

```
Answer Collection
      ↓
Label Assignment  (0 = Consistent, 1 = Hallucinated)
      ↓
Text Preprocessing  (case normalization, whitespace cleanup)
      ↓
TF-IDF Vectorization  (text → numerical feature vectors)
      ↓
Train-Test Split  (80% training / 20% testing)
      ↓
Model Training  (5 classifiers trained independently)
      ↓
Model Evaluation  (accuracy, AUC, confusion matrix, feature importance)
```

**Why TF-IDF instead of BERT/DeBERTa?**  
Transformer-based embeddings require access to large pretrained model repositories (Hugging Face), which were unavailable in the Kaggle environment used for this study. TF-IDF provides a practical, resource-efficient baseline and achieves competitive results — establishing a clear benchmark for future semantic approaches.

---

## 📈 Key Findings

- **All five models exceed 90% accuracy**, confirming that lexical features carry strong discriminative signal for this task.
- **XGBoost and LightGBM are highly correlated** (0.99 predicted probability correlation), as expected from their shared gradient-boosting mechanism.
- **Logistic Regression is the most distinct** from the boosting models (0.91–0.94 correlation), suggesting a potential gain from combining them in an ensemble.
- **Feature importance reveals a limitation**: the top features are common function words ("was", "both", "is", "the") rather than content-bearing terms — suggesting the models partly exploit stylistic patterns in hallucinated text rather than performing genuine fact verification.
- **The PCA scatter plot** shows substantial class overlap in 2D, confirming that the discriminative signal is distributed across many high-dimensional TF-IDF dimensions, not concentrated in a few principal directions.

---

## 📝 Citation

If you use this code or findings in your work, please cite:

```bibtex
@article{prawinkanna2024hallucination,
  title     = {Hallucination Detection and Reliability Assessment in Large Language Models Using Machine Learning Techniques},
  author    = {Prawin Kanna K and Naren K S and Lokeshwara},
  year      = {2024},
  institution = {Vellore Institute of Technology, Chennai},
  note      = {Guide: Dr. O. Vgnana Swathika}
}
```

---

## 👥 Authors

| Name | Email |
|---|---|
| Prawin Kanna K | prawin.2024@vitstudent.ac.in |
| Naren K S | naren.2024@vitstudent.ac.in |
| Lokeshwara | lokeshwara.2024@vitstudent.ac.in |

**Faculty Guide:** Dr. O. Vgnana Swathika  
**Institution:** Department of Computer Science and Engineering, VIT Chennai

---

## 📜 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- [HaluEval](https://github.com/RUCAIBox/HaluEval) — Li et al., EMNLP 2023
- [XGBoost](https://xgboost.readthedocs.io/) — Chen & Guestrin, KDD 2016
- [LightGBM](https://lightgbm.readthedocs.io/) — Ke et al., NeurIPS 2017
- [CatBoost](https://catboost.ai/) — Prokhorenkova et al., NeurIPS 2018
