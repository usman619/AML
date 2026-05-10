# Benchmark of Trojan Detection using Advance Machine Learning and Deep Learning techniques using open dataset


## Abstract
Trojan malware remains a critical cybersecurity threat by bypassing traditional perimeter defenses and exploiting network vulnerabilities. This research presents an advanced flow-based detection framework, utilizing a comprehensive dataset of approximately 177,000 instances to benchmark state-of-the-art machine learning (ML) and deep learning (DL) architectures. To address the massive variance inherent to network traffic, a rigorous preprocessing pipeline was implemented, encompassing domain-specific feature engineering, temporal dataset splitting to strictly prevent data leakage, and tree-based feature selection to isolate the 30 most deterministic variables. The study evaluated advanced supervised models including LightGBM, XGBoost, and a Temporal Convolutional Network (TCN) which achieved near-perfect classification metrics, recording accuracies exceeding 99.7% and AUC-ROC scores of 1.0000. Furthermore, to address the challenge of mutating or "zero-day" attacks, an Unsupervised Deep Autoencoder was developed. By applying Youden’s J statistic to dynamically calculate an optimal reconstruction error threshold, the anomaly detector achieved a 93.87% recall rate and an AUC of 0.9107 on entirely unseen malicious traffic. Rigorous statistical validation, adversarial noise robustness testing, and latency analyses (averaging under 30 microseconds per inference) confirm the models are highly calibrated and resistant to evasion. Ultimately, this study provides a comprehensive blueprint for model selection, threshold optimization, and feature engineering to deploy highly robust, real-time Trojan detection systems.

## Overview
- Datasets (CSV) are under `datasets/`.
- Notebooks are under `notebooks/` and include experiments with MLP, CNN, and MLP+LSTM models, plus dataset variants (no scaling, PCA, scaled).
- Improvements are inside `notebooks/improvement_notebooks` mainly `aml-improvement-ml-dl-models-v2.ipynb` file which forms on ML models (LightGBM and XGBoost) and DL models (TCN and Autoencoders).

## Models (notebooks)
- MLP: feed-forward multi-layer perceptron classifiers trained on flattened features. See `notebooks/pca_mlp_notebook.ipynb` and related notebooks for architecture and results.
- CNN: convolutional neural networks applied to sequence/image-like representations. See `notebooks/aml-trojan-detection-mlp-cnn.ipynb` and `notebooks/aml-trojan-detection-mlp-cnn-graphs.ipynb` for training and plots.
- MLP + LSTM: combined MLP and LSTM models for temporal feature modeling. See `notebooks/aml-trojan-detection-mlp-lstm.ipynb` and `notebooks/aml-trojan-detection-mlp-lstm-graphs.ipynb`.
- Improvement: `aml-improvement-ml-dl-models-v2.ipynb` file which forms on ML models (LightGBM and XGBoost) and DL models (TCN and Autoencoders).
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
### Before Improvement:

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

### After Improvement:
| Model | Accuracy | Precision | Recall | Specificity | F1-score | AUC-ROC |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| LightGBM | 0.9997 | 0.9998 | 0.9997 | 0.9998 | 0.9997 | 1.0000 |
| XGBoost | 0.9999 | 0.9997 | 1.0000 | 0.9997 | 0.9999 | 1.0000 |
| Temporal CNN (TCN) | 0.9974 | 0.9958 | 0.9992 | 0.9956 | 0.9975 | 1.0000 |
| Deep Autoencoder | 0.8612 | 0.8171 | 0.9387 | 0.7800 | 0.8737 | 0.9107 |

| Model | Brier Score (ECE) | Inference Latency | Noisy AUC-ROC | Performance Drop |
| :--- | :---: | :--- | :---: | :---: |
| LightGBM | _ | ~12.0 μs/sample | 0.9865 | 0.0135 |
| XGBoost | _ | ~15.0 μs/sample | 0.9785 | 0.0215 |
| Temporal CNN (TCN) | 0.0020 (ECE: 0.0018) | ~30.0 μs/sample | 0.9978 | 0.0022 |
| Deep Autoencoder | Unsupervised | ~16.0 μs/sample | 0.8925 | 0.0182 |
## Reproducibility notes
- Use the matching dataset variant (no scaling / scaled / pca) with the corresponding notebook (notebooks are named to indicate the preprocessing used).
- For GPU acceleration, ensure TensorFlow detects your GPU and install the appropriate CUDA/cuDNN drivers for your environment.

## Conclusion
This study proposed an extensive benchmarking system for Trojan detection, which is statistically validated and explicitly tackles important domain problems like temporal data leakage, feature redundancy, and preprocessing inconsistencies. Our experimental results show that data representation is essential for the model's performance; switching from generic dimensionality reduction (PCA) to tree-based feature selection and flattening network flows into 1D completely mitigated the model instability commonly observed in deep learning on tabular data. The optimized deep learning models, including the Temporal Convolutional Network (TCN), outperformed traditional models in classification tasks, with AUC scores reaching close to 1.0000, closely matching the performance of advanced gradient-boosted models (LightGBM, XGBoost).

Moreover, the research points towards the urgent need of unsupervised systems in zero-day threat hunting. The proposed Deep Autoencoder thus demonstrated a dynamic calibration of error thresholds of the reconstruction by using Youden's J and a recall rate of 93.87% on completely new malicious traffic. Finally, thorough testing of adversarial robustness and inference times of microseconds showed that these architectures are not just very accurate, but are actually feasible for line-rate, real-time deployment. Finally, this work provides a structured and repeatable feature engineering, model selection, and threshold optimization framework that helps develop the next generation of highly resilient and intelligent intrusion detection systems.

