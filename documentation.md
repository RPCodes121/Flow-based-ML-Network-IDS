# Machine Learning-Based Intrusion Detection: False Alarm Behaviour Analysis

## Abbreviations

ML- Machine Learning
IDS- Intrusion Detection System
FAR- False alarm rate

## Context

Network intrusion detection is a practice where attacks meant to break or gain unauthorized access inside a network are detected. A popular method by which IDSs are created is by using Machine Learning.

## Approach

The approach is to train a random forest binary classifier and calculate metrics such as accuracy and False alarm ratio.

## Dataset

The CICIDS2018 dataset is used which is a labeled dataset, specifically the subset which was created in 02-21-2018 and having Ddos attacks. A pickle file of the aforementioned processed subset is available in the repo. Link to dataset: https://www.kaggle.com/datasets/solarmainframe/ids-intrusion-csv?resource=download&select=02-16-2018.csv

## Methodology

First the dataset is loaded and the necessary cleaning is done (removing infinite and missing values). Benign class is labeled as 0 and the Ddos attack class is labeled 1. Initial Class imbalance study shows a higher attack to benign ratio i.e more amount of attacks than benign which is not the case in real-world network traffic. Hence by dividing the dataset into separate subsets for each class and reversing the imbalance , we acheive higher benign:attack ratio (Balancing methods like SMOTE are not used because network traffic is highly imbalanced by default). After this the top 15 features with the highest variances are captured and redundant features such as Dst Port (service context feature) and Timestamp are dropped. Then the dataset is divided among two variables X and Y , where X has only labels and Y has only labels. Then the dataset is splitted into Training and testing set.
