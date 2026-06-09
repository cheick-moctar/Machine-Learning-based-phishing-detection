# Machine Learning-Based Phishing Detection

A comparative study of machine learning models for automatic phishing website detection, developed for **MSCS 711 Computer Security**.

## Overview

This project develops and compares three machine learning models to detect phishing websites using 111 URL, domain, and page-based features extracted from 88,647 website samples.

| Model | Accuracy | Phishing Recall | False Negatives | Training Time |
|---|---|---|---|---|
| Decision Tree | 95.36% | 93% | 413 | ~1s |
| Random Forest | **97.06%** | **96%** | **249** | 13.9s |
| Neural Network | 96.20% | 94% | 355 | ~188s |

**Winner: Random Forest** — highest accuracy, best recall, fewest missed phishing sites.

---

## Dataset

- **Source:** [GregaVrbancic/Phishing-Dataset](https://github.com/GregaVrbancic/Phishing-Dataset)
- **Size:** 88,647 websites × 111 numerical features
- **Classes:** 58,000 legitimate (65.4%) · 30,647 phishing (34.6%)
- **Split:** 80% train (70,917) · 20% test (17,730), stratified

### Feature Categories
- **URL-based** — URL length, number of dots, hyphens, special characters
- **Domain-based** — domain activation time, nameservers, MX servers
- **Page-based** — SSL certificate, number of redirects, Google index status

---

## Models

### Decision Tree
Recursively partitions the feature space using Information Gain (entropy-based). Trained with default hyperparameters and `random_state=42`. Fast but prone to overfitting without depth restriction.

### Random Forest
Ensemble of 100 decision trees using **Bagging** + **Random Feature Selection** (√111 ≈ 10 features per split) with majority voting. Configured with `n_jobs=-1` for parallel training.

### Feedforward Neural Network
Built with TensorFlow/Keras:
- **Architecture:** Input(111) → Dense(128, ReLU) → Dropout(0.2) → Dense(64, ReLU) → Dropout(0.2) → Dense(32, ReLU) → Dense(1, Sigmoid)
- **Training:** Adam optimizer, Binary Cross-Entropy loss, Early Stopping (patience=5), max 50 epochs, batch size 32

---

## Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
tensorflow
```

Install all at once:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn tensorflow
```

---

## Usage

The notebook can be run directly in **Google Colab** — the dataset is loaded automatically from a public URL, no local download required.

```bash
# Clone the repo
git clone https://github.com/<your-username>/machine-learning-based-phising-detection.git
cd machine-learning-based-phising-detection

# Run the script
python phishing_detection.py
```

Or open in Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/10bRwE3RwBHUCpNcBXERY2OclayTYzptL)

---

## Key Findings

- **Random Forest** is the best model for structured phishing data — its ensemble mechanism corrects individual tree errors and naturally handles non-linear feature relationships.
- **Neural Networks** underperform on pre-engineered tabular features; they shine on raw unstructured data (images, text). Here they were 13× slower with lower accuracy.
- **Top predictive features** (from Random Forest importance): directory length, domain activation time, and dollar signs in the URL path — consistent with known attacker behavior of registering fresh domains with obfuscated paths.

---

## Project Structure

```
├── phishing_detection.py     # Full implementation (all models + visualizations)
├── report.pdf                # Full written report
└── README.md
```

---

## References

- APWG. (2024). *Phishing Activity Trends Report, Q4 2024.* https://apwg.org/trendsreports/
- Aleroud, A., & Zhou, L. (2017). Phishing environments, techniques, and countermeasures. *Computers & Security, 68*, 160–196.
- Mohammad, R., Thabtah, F., & McCluskey, L. (2014). Predicting phishing websites based on self-structuring neural network. *Neural Computing and Applications.*
- Alhogail, A. (2018). Applying machine learning to phishing email detection. *International Journal of Computer Applications.*
- Breiman, L. (2001). Random Forests. *Machine Learning, 45*(1), 5–32.
- Vrbancic, G. (2020). Phishing Dataset. https://github.com/GregaVrbancic/Phishing-Dataset

---

## Author

**Cheick Moctar Traore** · MSCS 711 Computer Security
