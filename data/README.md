# 📂 Data

This folder is where the HaluEval dataset should be placed before running the notebook.

## About the Dataset

**HaluEval** is a large-scale hallucination evaluation benchmark for Large Language Models, introduced by Li et al. (EMNLP 2023). Each instance contains:

| Column | Description |
|---|---|
| `knowledge` | Reference knowledge passage (ground truth context) |
| `raw_question` | The question derived from the knowledge passage |
| `correct_answer` | A factually correct answer |
| `hallucinated_answer` | A plausible but factually incorrect (hallucinated) answer |

In this project, the dataset is converted into a binary classification setup:
- Each `correct_answer` → **Label 0** (Factually Consistent)
- Each `hallucinated_answer` → **Label 1** (Hallucinated)

This produces **4,000 labeled instances** (2,000 per class) from the original dataset.

---

## How to Download

### Option 1 — Kaggle (recommended)
1. Go to [https://www.kaggle.com/datasets](https://www.kaggle.com/datasets) and search for **HaluEval**
2. Download `halueval.csv`
3. Place it in this `data/` folder

Or use the Kaggle API:
```bash
pip install kaggle
kaggle datasets download -d <dataset-slug>
```

### Option 2 — Hugging Face
```python
from datasets import load_dataset
ds = load_dataset("pminervini/HaluEval", "qa")
```

### Option 3 — Original GitHub
```
https://github.com/RUCAIBox/HaluEval
```

---

## Expected File

After downloading, your `data/` folder should contain:

```
data/
├── halueval.csv     ← place this here
└── README.md        ← this file
```

> **Note:** `halueval.csv` is excluded from version control via `.gitignore` due to file size. Do not commit large data files to GitHub.

---

## Reference

Li, J., Cheng, X., Zhao, W. X., Nie, J.-Y., & Wen, J.-R. (2023).  
*HaluEval: A Large-Scale Hallucination Evaluation Benchmark for Large Language Models.*  
Proceedings of EMNLP 2023.
