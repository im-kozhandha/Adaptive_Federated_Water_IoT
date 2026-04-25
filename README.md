# Federated Intrusion Detection for Industrial Water Treatment Systems

## Overview
A distributed intrusion detection system (IDS) for industrial IoT environments that leverages federated learning to detect cyber-physical attacks while preserving data privacy across edge nodes.

---

## Dataset

Due to size constraints and data governance considerations, the dataset is not included in this repository.

You can download the dataset from Kaggle:

👉 https://www.kaggle.com/datasets/vishala28/swat-dataset-secure-water-treatment-system

After downloading:
- Place the dataset inside a `data/` directory in the project root
- Ensure file paths match those used in the notebooks

---

## Problem Statement
Industrial water treatment systems generate highly sensitive sensor data that cannot be centralized due to privacy and operational constraints. At the same time:

- Cyber-attacks can cause **physical damage**
- Attack data is **rare (~3.79%)**, leading to imbalance
- Individual subsystems (edge nodes) have **limited visibility**

This project addresses how to build a **highly accurate, privacy-preserving IDS** using distributed learning.

---

## Approach / Methodology

### 1. Data Processing
- Removed timestamps to avoid temporal bias  
- Encoded labels: Normal (0), Attack (1)  
- Applied **StandardScaler (fit only on normal data)**  
- Retained missing values (handled implicitly)

---

### 2. Edge Node Simulation
- 51 sensors split across **6 edge nodes**
- Each node represents a **physical stage of the water plant**
- Non-overlapping feature subsets simulate real-world deployment

---

### 3. Local Model Training
- Model: **Random Forest (100 trees)**
- Each node:
  - Trains independently
  - Uses only its assigned sensors
  - Outputs feature importance vectors

---

### 4. Federated Learning

#### FedAvg (Baseline)
- Aggregates feature importance using simple averaging

#### AFEL (Adaptive Federated Edge Learning)
- Weights nodes based on **local F1-score**
- Performs **automatic feature selection (Top 20 features)**
- Reduces feature space by ~60%

---

### 5. Global Model
- Trained using aggregated feature importance
- Evaluated on a shared test set

---

## Tech Stack

| Category | Tools |
|----------|------|
| Language | Python |
| ML | Scikit-learn (Random Forest) |
| Data | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| Framework | Federated Learning (custom simulation) |

---

## Features

- Privacy-preserving training (no raw data sharing)
- Distributed edge learning architecture
- Adaptive aggregation (AFEL)
- Feature importance-based communication
- Dimensionality reduction without accuracy loss
- Robust to class imbalance
- Scalable for industrial IoT systems

---

## Results / Output

### Performance Summary

| Model | Accuracy | Precision | Recall | F1 Score |
|------|---------|----------|--------|---------|
| Edge Node Avg | ~0.99 | - | - | 0.9021 |
| FedAvg | **0.9999** | 0.9994 | 0.9988 | **0.9991** |
| AFEL | **0.9999** | 0.9995 | 0.9986 | **0.9991** |

---

### Key Insights

- **10.75% improvement in F1-score** vs individual nodes  
- Only **~20 errors out of 288k samples**  
- **60.8% feature reduction** with AFEL  
- Strong ROC & Precision-Recall performance under imbalance  

---

## Visual Outputs

The project generates multiple analytical visualizations:

| Category | Files |
|---------|------|
| Feature Importance | `Edge_Node_*_feature_importance.png`, `global_feature_importance.png` |
| Model Comparison | `final_model_comparison.png`, `edge_vs_global_comparison.png` |
| Performance Metrics | `precision_recall_curves.png`, `roc_curves_comparison.png` |
| Data Analysis | `sensor_correlation_heatmap.png`, `sensor_distributions.png` |
| Edge Analysis | `edge_node_accuracy_comparison.png`, `federated_vs_edge_accuracy.png` |

---

## How to Run

```bash
# Clone repository
git clone https://github.com/im-kozhandha/Adaptive_Federated_Water_IoT

# Install dependencies
pip install -r requirements.txt

# Download dataset from Kaggle and place in /data folder

# Run notebooks sequentially
cd notebooks

01_data_preprocessing.ipynb
02_edge_node_simulation.ipynb
03_edge_local_training.ipynb
04_federated_learning_simulation.ipynb
05_adaptive_federated_learning.ipynb
06_model_evaluation.ipynb
