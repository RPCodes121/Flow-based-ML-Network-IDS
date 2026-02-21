# Machine Learning-Based Intrusion Detection: False Alarm Behaviour Analysis

## Abbreviations

ML- Machine Learning
IDS- Intrusion Detection System
FAR- False alarm rate

## Context

Network intrusion detection is a practice where attacks meant to break or gain unauthorized access inside a network are detected. A popular method by which IDSs are created is by using Machine Learning. This project aims to focus on analyzing the high False alarm rate which is a common issue in ML-based IDS.

## Approach

The approach is to train a random forest binary classifier and calculate the FAR using the formula FP/(FP+TN).

## Dataset

The CICIDS2018 dataset is used which is a labeled dataset, specifically the subset which was created in 02-21-2018 and having Ddos attacks. A pickle file of the aforementioned processed subset is available in the repo

## Methodology

First the dataset is loaded and the necessary cleaning is done (removing infinite and missing values). Benign class is labeled as 0 and the Ddos attack class is labeled 1. Initial Class imbalance study shows a higher attack to benign ratio i.e more amount of attacks than benign which is not the case in real-world network traffic. Hence by dividing the dataset into separate subsets for each class and reversing the imbalance , we acheive higher benign:attack ratio (Balancing methods like SMOTE are not used)
