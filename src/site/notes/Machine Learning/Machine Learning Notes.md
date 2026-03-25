---
{"dg-publish":true,"permalink":"/machine-learning/machine-learning-notes/"}
---


##  Math Primer - Covered 
---

## Core Supervised Learning

### 2.1 Linear Regression
- OLS (Ordinary Least Squares)
- Cost function (MSE)
- Closed form solution
- Gradient descent solution
- Assumptions (linearity, homoscedasticity, no multicollinearity)
- R-squared, adjusted R-squared

> 📚 ISLR (Ch 3), ESL (Ch 3)




---

### 2.2 Regularization
- Why regularization (overfitting)
- Ridge regression (L2) — shrinks coefficients
- Lasso regression (L1) — shrinks to zero, feature selection
- ElasticNet — combination of L1 + L2
- Bias-variance tradeoff

> 📚 ISLR (Ch 6), ESL (Ch 3)

---

### 2.3 Logistic Regression
- Sigmoid function
- Log-odds interpretation
- Binary cross-entropy loss
- Decision boundary
- Multiclass extension (softmax)

> 📚 ISLR (Ch 4), ESL (Ch 4)

---

### 2.4 Decision Trees
- Splitting criteria (Gini, entropy, information gain)
- Recursive partitioning
- Overfitting in trees
- Pruning
- Depth vs complexity tradeoff

> 📚 ISLR (Ch 8), ESL (Ch 9)

---

### 2.5 Ensemble Methods
- Bagging — reduce variance
- Random Forest — bagging + feature randomness
- Boosting — reduce bias sequentially
- AdaBoost
- Gradient Boosting (GBM)
- XGBoost — regularized GBM
- LightGBM — histogram-based, faster
- CatBoost — handles categoricals natively
- Stacking

> 📚 ISLR (Ch 8), ESL (Ch 10), XGBoost paper

---

### 2.6 Support Vector Machines
- Maximum margin classifier
- Support vectors
- Soft margin (C parameter)
- Kernel trick (RBF, polynomial, linear)
- SVM for classification and regression (SVR)

> 📚 ISLR (Ch 9), ESL (Ch 12)

---

### 2.7 K-Nearest Neighbors
- Distance metrics (Euclidean, Manhattan, cosine)
- K selection and bias-variance tradeoff
- Curse of dimensionality
- When KNN works and when it fails

> 📚 ISLR (Ch 2), ESL (Ch 2)

---

### 2.8 Naive Bayes
- Bayes theorem applied to classification
- Conditional independence assumption
- Gaussian NB, Multinomial NB, Bernoulli NB
- Why "naive" and when it still works

> 📚 Bishop (Ch 4)

---

##  Unsupervised Learning

### 3.1 Clustering
- K-Means — algorithm, objective, limitations
- K-Means++ initialization
- Elbow method for K selection
- Hierarchical clustering (agglomerative, divisive)
- Dendrograms
- DBSCAN — density-based, handles noise
- Cluster evaluation (silhouette score, inertia)

> 📚 ISLR (Ch 12), ESL (Ch 14)

---

### 3.2 Dimensionality Reduction
- PCA — principal components, explained variance
- How PCA uses eigendecomposition
- Scree plot
- t-SNE — nonlinear, visualization
- UMAP — faster than t-SNE, preserves structure
- LDA (Linear Discriminant Analysis) — supervised DR

> 📚 ISLR (Ch 12), ESL (Ch 14), Bishop (Ch 12)

---

### 3.3 Anomaly Detection
- Statistical methods (Z-score, IQR)
- Isolation Forest
- One-class SVM
- Autoencoders for anomaly detection
- Local Outlier Factor (LOF)

> 📚 ESL (Ch 14), Géron (Ch 9)

---

### 3.4 Association Rules
- Apriori algorithm
- Support, confidence, lift
- Market basket analysis

> 📚 ESL (Ch 14)

---

##  Model Evaluation & Selection

### 4.1 Bias-Variance Tradeoff
- Bias — underfitting, too simple
- Variance — overfitting, too complex
- Total error = bias² + variance + irreducible noise
- How regularization, depth, ensemble affect this

> 📚 ISLR (Ch 2), ESL (Ch 7)

---

### 4.2 Cross-Validation
- Train/val/test split — why three sets
- K-fold cross-validation
- Stratified K-fold
- Leave-one-out CV (LOOCV)
- Time series CV (no data leakage)

> 📚 ISLR (Ch 5), ESL (Ch 7)

---

### 4.3 Classification Metrics
- Confusion matrix (TP, TN, FP, FN)
- Accuracy — when it lies
- Precision — of predicted positives, how many correct
- Recall (sensitivity) — of actual positiv
  