# Iris Dataset Classification using Apache Spark MLlib

## Overview

This project demonstrates a complete machine learning workflow using **Apache Spark MLlib** to classify Iris flowers into three species (Setosa, Versicolor, and Virginica) based on sepal and petal measurements. The analysis compares four classification algorithms using both **manual grid search** and **automated hyperparameter tuning** approaches, providing insights into model performance and tuning methodology effectiveness.

**Student**: P166248 - Suraya Balqis binti Ab.Ghafar  
**Course**: STQD6324 Data Management — Assignment 1  
**Platform**: Databricks with Serverless Compute

### Assignment1.ipynb - Full Notebook without Outputs, to run via Databricks
### Assignment1_withOutput.ipynb - Full Notebook with Outputs
---

## Dataset and Methodology

### Dataset
- **Source**: scikit-learn's built-in Iris dataset (Fisher, 1936)
- **Size**: 150 samples with 4 numeric features
  - Sepal length (cm)
  - Sepal width (cm)
  - Petal length (cm)
  - Petal width (cm)
- **Target**: 3 balanced classes (50 samples each)
  - Class 0: Setosa
  - Class 1: Versicolor
  - Class 2: Virginica
- **Split**: 70% training (99 samples) / 30% testing (51 samples)

### Exploratory Data Analysis
Statistical analysis revealed:
- Balanced class distribution (no imbalance issues)
- No missing values or data quality concerns
- Strong correlation between petal length and petal width (0.96)
- Setosa is linearly separable from other classes

### Models Evaluated
1. **Random Forest Classifier**
   - Parameters tuned: numTrees, maxDepth
   - Tuning method: Manual grid search + TrainValidationSplit

2. **Logistic Regression**
   - Parameters tuned: regParam, maxIter
   - Tuning method: Manual grid search + CrossValidator (3-fold)

3. **Decision Tree**
   - Parameters tuned: maxDepth, minInstancesPerNode
   - Tuning method: Manual grid search + TrainValidationSplit

4. **Multilayer Perceptron (Neural Network)**
   - Parameters tuned: layer architectures
   - Tuning method: Manual grid search + CrossValidator (3-fold)

### Hyperparameter Tuning Approaches
- **Manual Grid Search**: Systematic testing of predefined parameter combinations with validation split (80/20 of training data)
- **Automated Tuning**: CrossValidator and TrainValidationSplit for efficient parameter optimization

---

## Results and Key Findings

### Model Performance

**Manual Grid Search Results:**

| Rank | Model | Accuracy | Precision | Recall | F1 Score |
|------|-------|----------|-----------|--------|----------|
| 1 | Logistic Regression | 98.04% | 98.14% | 98.04% | 98.02% |
| 1 | Multilayer Perceptron | 98.04% | 98.14% | 98.04% | 98.02% |
| 3 | Random Forest | 96.08% | 96.08% | 96.08% | 96.08% |
| 4 | Decision Tree | 94.12% | 94.13% | 94.12% | 94.06% |

- Average F1 Score: 96.54%
- Standard Deviation: 0.0189

**Automated Tuning Results:**

All four models achieved identical performance with automated tuning:
- Accuracy: 98.04%
- F1 Score: 98.02%
- Standard Deviation: 0.0000 (complete convergence)

### Key Findings

1. **Automated tuning outperformed manual tuning**: While manual grid search showed performance variation (94-98% F1), automated methods (CrossValidator and TrainValidationSplit) achieved uniform 98.02% F1 across all models with zero variance.

2. **Simple models matched complex models**: Logistic Regression (linear model) achieved the same performance as Multilayer Perceptron (neural network), demonstrating that model complexity is not always necessary for linearly separable datasets.

3. **Dataset characteristics influenced results**: The relatively simple Iris dataset, with clear class separation and balanced distribution, allowed all well-tuned models to converge to near-optimal performance.

4. **Classification errors were predictable**: All misclassifications occurred between Versicolor and Virginica (the two overlapping classes), with Setosa being perfectly separable.

### Limitations
- Small dataset size (150 samples) limits generalizability
- Single train/test split may not capture full performance variance
- Results specific to the Iris dataset and may not extend to more complex classification problems

---

## Instructions to Reproduce

### Prerequisites
- Databricks workspace (Free Edition or higher) https://www.databricks.com/
- Login required
- Serverless compute with CPU
- Pre-installed libraries (included by default):
  - `pyspark.ml`
  - `scikit-learn`
  - `pandas`, `matplotlib`, `seaborn`, `numpy`

### Steps to Execute

1. **Open the notebook** in Databricks:
   - Navigate to `/Users/surayaqis@gmail.com/DataManagement/Assignment1`

2. **Attach to Serverless Compute**:
   - Click "Connect" in the top-right corner
   - Select "Serverless" compute option

3. **Run all cells sequentially**:
   - Click "Run All" to execute all 54 cells in order
   - **Important**: Do not skip cells or run out of order due to variable dependencies

4. **Review results**:
   - Section 8: Model comparison tables and statistical analysis
   - Section 9: Sample predictions with error analysis

### Execution Time
- Complete execution takes approximately 10-15 minutes on Serverless compute
- Automated tuning sections (CrossValidator and TrainValidationSplit) are the most time-intensive

### Expected Outputs
When executed successfully, you should observe:
- 150 samples loaded with balanced class distribution
- All models achieving >94% F1 score
- Automated tuning converging to 98.02% F1 across all models
- Minimal misclassifications (only between Versicolor and Virginica)

---

## References

- Fisher, R.A. (1936). The use of multiple measurements in taxonomic problems. *Annals of Eugenics*, 7(2), 179-188.
- Apache Spark MLlib Documentation: https://spark.apache.org/docs/latest/ml-classification-regression.html

---
