# AI-Powered Intrusion Detection System

## Information Security – Assignment 1

An AI-powered Network Intrusion Detection System (NIDS) that uses Machine Learning to classify network traffic as **Normal** or **Malicious** using the UNSW-NB15 dataset.

---

## Project Overview

The objective of this project is to develop a Machine Learning-based Intrusion Detection System capable of identifying potentially malicious network traffic.

The project simulates a real-world security scenario in which an organization uses Machine Learning as an additional intelligent layer alongside traditional network security mechanisms.

The system performs:

- Network traffic data analysis
- Exploratory Data Analysis (EDA)
- Data preprocessing
- Categorical feature encoding
- Numerical feature standardization
- Machine Learning classification
- Performance evaluation
- Feature importance analysis
- Real-time threat detection simulation

---

## Real-World Scenario

Organizations continuously receive network traffic from users, servers, applications, and external systems. Some of this traffic may contain malicious activities such as reconnaissance, denial-of-service attacks, exploits, fuzzing, and other intrusion attempts.

Traditional security mechanisms such as firewalls and rule-based detection systems may not identify every evolving attack pattern.

This project demonstrates how a Machine Learning-based NIDS can analyze network traffic and classify individual connections as:

- `0` – Normal Traffic
- `1` – Malicious/Attack Traffic

The solution is designed as a Proof-of-Concept (PoC) for augmenting a traditional NIDS.

---

## Dataset

### UNSW-NB15

The project uses the **UNSW-NB15** network intrusion detection dataset.

The dataset contains normal network traffic as well as multiple categories of malicious network activity. It provides network-flow features including protocol information, connection state, packet statistics, byte counts, timing information, and other traffic characteristics.

The officially provided training and testing datasets are used:

- `UNSW_NB15_training-set.csv`
- `UNSW_NB15_testing-set.csv`

The predefined training and testing split allows the model to be evaluated on previously unseen network traffic.

### Attack Categories

The UNSW-NB15 dataset contains multiple attack categories, including:

- Fuzzers
- Analysis
- Backdoors
- DoS
- Exploits
- Generic
- Reconnaissance
- Shellcode
- Worms

The dataset is not included in this repository because of its large size. Users should obtain the dataset from the official UNSW-NB15 dataset source.

Official Dataset Source:

https://research.unsw.edu.au/projects/unsw-nb15-dataset

---

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook
- Google Colab

---

## Machine Learning Model

### Random Forest Classifier

A **Random Forest Classifier** was selected as the primary Machine Learning model.

Random Forest is suitable for network traffic classification because it can model complex relationships between multiple network-flow features and is effective for classification tasks involving numerical and encoded categorical features.

The model is trained using the provided training dataset and evaluated on the separate unseen testing dataset.

The primary security objective is to achieve a high recall for malicious traffic because false negatives represent attacks that were not detected by the system.

---

## Data Preprocessing

The following preprocessing steps are performed:

1. Separation of target and input features.
2. Removal of the `id` identifier column.
3. Removal of `attack_cat` from model features to prevent target-related information from being used during classification.
4. Identification of numerical and categorical features.
5. Standardization of numerical features using `StandardScaler`.
6. One-hot encoding of categorical features using `OneHotEncoder`.
7. Handling of previously unseen categorical values using `handle_unknown="ignore"`.
8. Transformation of the training and testing data using the same preprocessing pipeline.

The final preprocessed dataset contains **194 features**.

---

## Exploratory Data Analysis

EDA was performed to understand the characteristics of the network traffic and attack distribution.

The analysis includes:

- Dataset structure and information
- Missing value analysis
- Statistical summary
- Normal vs. malicious traffic distribution
- Attack category distribution
- Categorical feature analysis
- Training and testing dataset structure

---

## Model Evaluation

The Random Forest model was evaluated using the unseen UNSW-NB15 testing dataset.

### Results

| Metric | Result |
|---|---:|
| Accuracy | 87.11% |
| Precision | 81.78% |
| Attack Recall | 98.54% |
| False Positives | 9,955 |
| False Negatives | 660 |

### Classification Performance

The model achieved a high recall for malicious traffic, meaning that it successfully detected most of the attacks present in the testing dataset.

The high attack recall is particularly important in an intrusion detection scenario because false negatives represent malicious connections that remain undetected.

---

## Confusion Matrix

The model produced the following confusion matrix:

| | Predicted Normal | Predicted Attack |
|---|---:|---:|
| Actual Normal | 27,045 | 9,955 |
| Actual Attack | 660 | 44,672 |

### Security Interpretation

- **True Negatives (TN):** 27,045 normal connections were correctly classified.
- **True Positives (TP):** 44,672 malicious connections were correctly detected.
- **False Positives (FP):** 9,955 normal connections were incorrectly classified as attacks.
- **False Negatives (FN):** 660 malicious connections were incorrectly classified as normal.

The relatively small number of false negatives demonstrates strong attack detection capability. However, the number of false positives indicates that further tuning would be required before deploying the model in a production environment.

---

## Feature Importance

Random Forest feature importance was analyzed to determine which network traffic characteristics contributed most to the classification decisions.

Some of the most important features included:

- `sttl`
- `ct_state_ttl`
- `dload`
- `rate`
- `dttl`
- `sload`
- `synack`
- `tcprtt`
- `sbytes`
- `ct_srv_dst`

These features provide information related to traffic behavior, packet/byte rates, connection characteristics, and timing, which can help distinguish normal and malicious network activity.

---

## Real-Time Threat Detection

A real-time threat detection simulation is included in the notebook.

Instead of processing the complete testing dataset at once, the system can receive a single network traffic record and perform inference using the trained Random Forest model.

The process is:

```text
Incoming Network Traffic
          ↓
Data Preprocessing
          ↓
Feature Transformation
          ↓
Random Forest Model
          ↓
Prediction
          ↓
NORMAL / MALICIOUS
          ↓
Attack Probability
