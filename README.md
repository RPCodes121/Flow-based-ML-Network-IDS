# Machine Learning-Based Network Intrusion Detection System

## Overview

This project implements a **flow-based Network Intrusion Detection System (IDS)** using machine learning.  
The system analyzes statistical features extracted from network traffic flows and determines whether a connection is **benign or malicious**.

The goal of this project is not to build a state-of-the-art detector, but to demonstrate a **complete, correct ML pipeline on real cybersecurity data** and evaluate the system using meaningful IDS metrics rather than only accuracy.

---

## Important Note

This project demonstrates a **baseline ML intrusion detector on a benchmark dataset**.  
Performance on real enterprise networks will differ due to traffic diversity, encrypted protocols, and environmental noise.

## Dataset

The model is trained and evaluated using the **CICIDS2018** intrusion detection dataset, specifically on the dataset created on February 21, 2018.

The dataset provides network flow statistics such as:

- packet lengths
- flow duration
- header flag counts
- window sizes
- inter-arrival timing information

For detection purposes, all attack categories were grouped into a single class:

- `0 → Benign`
- `1 → Attack`

This converts the task into **intrusion detection** instead of attack classification.

.

---

## Preprocessing

Real network datasets contain artifacts and inconsistencies.  
The following preprocessing steps were applied:

1. Removed NaN and infinite values
2. Removed duplicate flows
3. Converted labels to binary
4. Train-test split
5. Feature selection (statistical)
6. Removed contextual leakage features

### Leakage Handling

Feature importance analysis revealed that certain contextual attributes (notably destination port) could produce unrealistically perfect classification because the model learns _which service is attacked_ rather than _malicious behavior_.

The destination port feature was therefore removed to ensure the model learns traffic patterns instead of service identity.

---

## Models

### Baseline Model — Logistic Regression

A linear classifier is used as the primary IDS baseline.  
This model provides realistic behavior and avoids memorizing dataset-specific patterns.

The baseline detector predicts whether a flow should generate a security alert.

### Comparison Model — Random Forest

A Random Forest classifier was also trained for comparison.  
The tree-based model achieved near-perfect classification. However, feature importance analysis showed strong dataset separability, indicating the model may exploit dataset-specific traffic characteristics rather than general intrusion behavior.

For this reason, Logistic Regression is treated as the primary evaluation model.

---

## Validation and Reliability Checks

Because intrusion detection datasets can produce misleadingly high accuracy, additional checks were performed to verify the results were meaningful.

### Overfitting Check

Training and testing performance were compared.  
If the model were memorizing samples, training accuracy would be significantly higher than testing accuracy. Instead, both were similar. This indicated the model was learning consistent patterns in the data rather than memorizing individual flows.

### Leakage Check

Feature importance analysis revealed the **destination port** had a very strong influence on predictions.  
This is a contextual attribute (identifies the targeted service) rather than a behavioral indicator of malicious traffic. Such features can cause artificially perfect classification.

To prevent this, the destination port feature was removed and the model retrained.

### Model Capacity Comparison

Two model families were evaluated:

- Logistic Regression (linear baseline)
- Random Forest (non-linear ensemble)

The Random Forest achieved near-perfect classification, while the linear model produced realistic errors.  
This suggests the dataset contains strongly separable statistical patterns. The linear model was therefore used as the primary baseline to obtain more realistic intrusion detection behavior.

## Evaluation Metrics

Accuracy alone is misleading in intrusion detection systems.  
Instead, the system is evaluated using operational metrics:

### Detection Rate (Recall)

\[
Detection\ Rate = \frac{TP}{TP + FN}
\]

Measures how many attacks are successfully detected.

### False Alarm Rate (FAR)

\[
FAR = \frac{FP}{FP + TN}
\]

Measures how often benign traffic incorrectly triggers alerts.

Low FAR is critical because excessive alerts overwhelm security analysts.

---

## Results

The baseline model successfully detected malicious traffic while maintaining a very low false alarm rate.

Interpretation:

- The model can distinguish attack patterns in the dataset
- The dataset exhibits strong separability
- Evaluation beyond raw accuracy is necessary for IDS research

---

## Requirements

Install dependencies:
pip install -r requirements.txt

## How to Run

1. Open `preprocess_eda.ipynb`.Run all cells sequentially. The notebook will:
   - load the dataset
   - preprocess data
   - Perform EDA
   - Generate a pickle file of the processed data
2. After generating the processed dataset, open `model_training_logreg.ipynb` for Logistic Regression or `model_training_rfc.ipynb` for random forest. Both the notebooks will:
   - Load the processed dataset
     -split the dataset into training and testing set
     -select the K best features in the training set by ranking features based on their variance.
     -Train the respective models
     -generate accuracy score, confusion matrix , False alarm rate and detection accuracy.
