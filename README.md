# Benchmark of Trojan Detection using Advance Machine Learning and Deep Learning techniques using open dataset


## Abstract
Trojan malware has been a significant cybersecurity threat because it bypasses the traditional detection and exploits network vulnerabilities. This research establishes an extensive benchmarking system for detecting Trojans by utilizing machine learning and deep learning, on trojan detection dataset consisting of 86 features and around 170,000 instances. The methodology included a thorough preprocessing workflow which included cleaning of data, elimination of leakage, extraction of temporal features and feature engineering. Moreover, the research has examined redundancy of features and its attempt to optimize the performance of the models. Some of the algorithms we have used are Random Forest, XGBoost, MLP, LSTM and hybrid frameworks. The results suggest that tree-based algorithms outperform deep learning-based algorithms on structured datasets, with the highest accuracy of 0.8442 and the highest AUC-ROC of 0.9289. Among the deep learning models, the Multi-Layer Perceptron (MLP) performed best with a compressed feature set as it recorded the highest accuracy at 0.7828, F1-score at 0.8024 and AUC-ROC at 0.8642. Hybrid architectures also showed high recall rates up to 0.9535 and this proved to be of advantage when it comes to security-sensitive applications. In the end, the study provides valuable insights on the choice of models, feature engineering approaches, and testing procedures of creating robust Trojan detection systems.

## Overview
- Datasets (CSV) are under `datasets/`.
- Notebooks are under `notebooks/` and include experiments with MLP, CNN, and MLP+LSTM models, plus dataset variants (no scaling, PCA, scaled).

## Models (notebooks)
- MLP: feed-forward multi-layer perceptron classifiers trained on flattened features. See `notebooks/pca_mlp_notebook.ipynb` and related notebooks for architecture and results.
- CNN: convolutional neural networks applied to sequence/image-like representations. See `notebooks/aml-trojan-detection-mlp-cnn.ipynb` and `notebooks/aml-trojan-detection-mlp-cnn-graphs.ipynb` for training and plots.
- MLP + LSTM: combined MLP and LSTM models for temporal feature modeling. See `notebooks/aml-trojan-detection-mlp-lstm.ipynb` and `notebooks/aml-trojan-detection-mlp-lstm-graphs.ipynb`.

## Dataset variants
- `datasets/trojan_cleaned(no_scaling).csv` — raw features without scaling.
- `datasets/trojan_cleaned(pca).csv` — PCA-reduced features.
- `datasets/trojan_cleaned(scaled).csv` — scaled features (e.g., StandardScaler).

## Quick start
1. Create and activate a virtual environment (recommended):

```bash
python3 -m venv .venv
source .venv/bin/activate
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```


3. Open a notebook from the `notebooks/` folder and run cells. Datasets are loaded from the `datasets/` directory update paths in the notebooks(as we ran it on Kaggle).

## Dataset used:
The dataset used for this research paper is a Trojan Detection dataset available on Kaggle.
Link: https://doi.org/10.34740/kaggle/dsv/2625272

## Results
| Model | Preprocessing | Accuracy | F1-score | Recall | AUC-ROC |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Random Forest | No Scaling | 0.8442 | 0.8525 | 0.8794 | 0.9289 |
| XGBoost | No Scaling | 0.8044 | 0.8309 | 0.9382 | 0.8921 |
| Logistic Regression | PCA | 0.7581 | 0.7895 | 0.8858 | 0.8071 |
| SVM | PCA | 0.5376 | 0.6621 | 0.8848 | 0.5720 |
| KNN | RobustScaler | 0.6947 | 0.7033 | 0.7064 | 0.6944 |
| MLP | PCA | 0.7828 | 0.8024 | 0.8615 | 0.8642 |
| MLP + LSTM | Hybrid | 0.7169 | 0.7749 | 0.9535 | 0.7826 |
| MLP + CNN | Hybrid | 0.7169 | 0.7749 | 0.9535 | 0.7826 |

## Reproducibility notes
- Use the matching dataset variant (no scaling / scaled / pca) with the corresponding notebook (notebooks are named to indicate the preprocessing used).
- For GPU acceleration, ensure TensorFlow detects your GPU and install the appropriate CUDA/cuDNN drivers for your environment.

## Files of interest
- Notebooks: see `notebooks/` (multiple notebooks for MLP, CNN, LSTM, and dataset variants).
- Datasets: see `datasets/` (CSV files described above).
