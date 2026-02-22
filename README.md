# ML-Based Network Intrusion Detection System

## Overview

This project demonstrates an end-to-end machine learning pipeline applied to network security data.  
The system analyzes network flow statistics and classifies traffic as **benign or malicious**.

---

## Dataset

- CICIDS2018 flow-based intrusion detection dataset (specifically the subset titled 02-21-2018)
- Multiple statistical features per network flow (packet size, duration, flags, timing)
- Converted to binary detection:
  - `0 = benign`
  - `1 = attack`

A processed sample in .pkl is included for reproducibility.

---

## Pipeline

1. Data cleaning (NaN/inf removal, duplicates)
2. Binary label preparation
3. Removal of contextual leakage feature (`Dst Port`)
4. Train/test split
5. Feature selection
6. Model training
7. Evaluation

---

## Models

An initial tree-based model (Random Forest) was evaluated to understand dataset behavior.  
It achieved near-perfect classification, which prompted additional validation checks.

After verifying feature influence and evaluation methodology, a linear baseline model (Logistic Regression) was used as the primary detector to obtain more realistic intrusion detection performance.

---

## Evaluation

Instead of accuracy alone, the system uses operational IDS metrics:

- **Detection Rate (Recall):** ability to catch attacks
- **False Alarm Rate (FAR):** frequency of false alerts

This better reflects practical intrusion detection usefulness.

---

## Key Observation

Feature importance showed the destination port could dominate predictions.  
This was removed because it identifies the attacked service rather than malicious behavior, which can produce misleadingly perfect results.

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
