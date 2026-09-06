# Capstone-Project
CAU-Fusion is a reliability-aware multimodal AI framework for diabetic retinopathy prediction, combining clinical data using CatBoost and retinal fundus images using DenseNet-121. It improves predictive reliability by considering confidence, agreement, disagreement, and uncertainty during multimodal fusion.
# CAU-Fusion
### A Framework for Quantifying Confidence, Agreement, and Uncertainty in Predictive Fusion

CAU-Fusion is a reliability-aware multimodal predictive fusion framework designed to improve the trustworthiness and interpretability of AI-assisted medical prediction.

The project addresses a key limitation of conventional multimodal prediction systems: most existing fusion approaches combine model probabilities using simple averaging or weighted averaging without explicitly considering how reliable each prediction is.

To address this limitation, CAU-Fusion integrates predictions from two complementary medical data modalities:

- Structured clinical data
- Retinal fundus images

For clinical data, CatBoost is used to learn patterns from demographic, metabolic, renal, and lipid-related clinical features. For retinal fundus image analysis, DenseNet-121 is used to extract visual features and generate image-based prediction probabilities.

The outputs of these two independent models are then processed by the proposed CAU-Fusion mechanism. Instead of treating both predictions as equally reliable, the framework explicitly evaluates:

- Confidence
- Agreement
- Disagreement
- Uncertainty

These reliability-related factors are incorporated into the final fusion process to strengthen reliable predictions while reducing the influence of conflicting or uncertain evidence.

---

## Research Motivation

Multimodal medical prediction can provide more comprehensive information by combining different sources of evidence. Clinical records contain structured information about a patient's health condition, while retinal fundus images provide visual information that may contain disease-related characteristics.

However, simply averaging the probabilities produced by different models can produce misleadingly confident predictions when the modalities disagree.

For example, if the clinical model produces a high-risk prediction while the retinal image model produces a low-risk prediction, conventional averaging may still produce a confident final result without indicating that the underlying evidence is conflicting.

CAU-Fusion addresses this problem by explicitly quantifying the reliability of the available evidence before generating the final prediction.

---

## Main Objective

The primary objective of CAU-Fusion is to develop a general-purpose reliability-aware predictive fusion framework that improves the trustworthiness of multimodal medical prediction by incorporating:

- Prediction Confidence
- Inter-modal Agreement
- Prediction Disagreement
- Entropy-based Uncertainty

The framework is demonstrated using diabetic retinopathy prediction as a real-world medical case study.

---

## System Overview

The framework consists of three major components:

### 1. Clinical Prediction Model — CatBoost

CatBoost is used to analyze structured clinical information.

The clinical dataset includes features such as:

- Age
- Gender
- Urea
- Creatinine
- HbA1c
- BMI
- Cholesterol
- Triglycerides (TG)
- HDL
- LDL
- VLDL

The model generates a clinical prediction probability:

`Pc`

CatBoost was selected after comparing multiple machine learning models, including Logistic Regression, Random Forest, XGBoost, and MLP.

---

### 2. Retinal Image Prediction Model — DenseNet-121

DenseNet-121 is used for retinal fundus image analysis.

The retinal images are preprocessed by:

- Resizing images to 224 × 224 pixels
- Pixel normalization
- Rotation augmentation
- Zoom augmentation
- Horizontal flipping

DenseNet-121 generates the image-based prediction probability:

`Pi`

DenseNet-121 was selected based on its balanced performance, high recall, AUC-ROC, stable convergence, and generalization performance among the evaluated CNN architectures.

---

### 3. CAU-Fusion

The clinical probability `Pc` and image probability `Pi` are passed into the proposed CAU-Fusion framework.

The fusion process consists of:

1. Probability Calibration
2. Simple Fusion
3. Agreement Calculation
4. Confidence Estimation
5. Disagreement Measurement
6. Entropy-based Uncertainty Estimation
7. Final Reliability-aware Fusion
8. Hyperparameter Optimization
9. Decision Threshold Optimization

The final probability is represented as:

`Pfinal`

The framework increases the contribution of reliable and consistent predictions while penalizing conflicting and uncertain predictions.

---

## Fusion Strategy

CAU-Fusion uses a reliability-aware fusion formulation:

`Pfinal = (S × (1 + γ × Cf)) / (1 + α × D + β × U + ε)`

Where:

- `S` = Base fusion score
- `Cf` = Confidence score
- `D` = Disagreement between modalities
- `U` = Entropy-based uncertainty
- `α` = Disagreement penalty weight
- `β` = Uncertainty penalty weight
- `γ` = Confidence boosting weight
- `ε` = Small constant to prevent division by zero

The main idea is simple:

**Reliable + Agreeing predictions → stronger fusion**

**Conflicting predictions → higher disagreement penalty**

**Ambiguous predictions → higher uncertainty penalty**

This allows the framework to become more cautious when the available evidence is inconsistent.

---

## Data Processing Pipeline

The complete workflow follows two parallel prediction pipelines.

### Clinical Pipeline

```text
Clinical Dataset
      ↓
Data Exploration
      ↓
Data Cleaning
      ↓
Missing Value Handling
      ↓
Categorical Encoding
      ↓
Class Balancing
      ↓
Model Training
      ↓
CatBoost
      ↓
Clinical Probability (Pc)



Retinal Image Pipeline: 

Retinal Fundus Images
      ↓
Image Exploration
      ↓
Image Resizing
      ↓
Normalization
      ↓
Data Augmentation
      ↓
CNN Training
      ↓
DenseNet-121
      ↓
Image Probability (Pi)


Fusion Pipeline:
Clinical Probability (Pc)
          +
Image Probability (Pi)
          ↓
Probability Calibration
          ↓
Simple Fusion
          ↓
Agreement
          ↓
Confidence
          ↓
Disagreement
          ↓
Uncertainty
          ↓
CAU-Fusion
          ↓
Final Probability (Pfinal)
          ↓
Final Prediction

Key Features
Multimodal medical prediction
Clinical data analysis
Retinal fundus image analysis
CatBoost-based clinical prediction
DenseNet-121-based image prediction
Probability calibration
Confidence-aware fusion
Agreement analysis
Disagreement modeling
Entropy-based uncertainty estimation
Reliability-aware decision fusion
Hyperparameter optimization
Decision-threshold optimization
Explainable fusion components
Risk-level interpretation
Support for manual review of uncertain/conflicting cases
