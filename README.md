# Ensemble Learning - Stacking Explained  
## Improving Predictions by Combining Models

This project explores **stacking**, a powerful ensemble learning technique that boosts model performance by **combining multiple models**. Instead of relying on a single model, stacking lets different models **work together**, with a final model (meta-learner) making the best possible prediction.  

Here, I apply stacking to a **house price prediction** problem, but the concept is useful for many types of regression and classification tasks.

---

## What is Stacking?
Stacking is an **ensemble learning** technique that uses multiple models to make predictions and then combines those predictions using another model. This final model—the **meta-learner**—figures out the best way to weigh and use the individual models' outputs.

### **How It Works**
1. **Train Base Models**  
   - Train different models on the dataset (e.g., Linear Regression, Random Forest, Gradient Boosting).  
2. **Generate Predictions**  
   - Each base model makes predictions on the dataset. These predictions are not used directly but instead serve as new inputs.  
3. **Train a Meta-Model**  
   - A meta-model (like Ridge Regression) learns how to combine these predictions optimally.  
4. **Final Prediction**  
   - The meta-model produces the final, improved prediction.

Here’s a simple **visual representation of the stacking process**:

                     Raw Data
                        │
                        ▼
    ┌──────────────────────────────────────────┐
    │ Train Base Models (Linear, RF, XGB, LGB) │
    └──────────────────────────────────────────┘
                        │
                        ▼
     ┌───────────────────────────────────────┐
     │ Base Model Predictions (New Features) │
     └───────────────────────────────────────┘                   
                        │
                        ▼
      ┌─────────────────────────────────────┐
      │ Train Meta-Model (Ridge Regression) │
      └─────────────────────────────────────┘
                        │
                        ▼
                 Final Prediction

---

## How Stacking Was Implemented in This Project

### **Step 1: Train Individual Models**  
The following models were used as base learners:  
- **Linear Regression** – Simple, interpretable baseline.  
- **Lasso Regression** – Helps with feature selection.  
- **Random Forest Regressor** – Captures non-linearity and interactions.  
- **Gradient Boosting (XGBoost & LightGBM)** – Handles complex patterns efficiently.  

### **Step 2: Generate Out-of-Fold Predictions**  
To avoid **data leakage**, I used cross-validation to generate **out-of-fold predictions** for each model. These predictions were then used as input features for the meta-learner.

### **Step 3: Train the Meta-Model**  
I used **Ridge Regression** as the meta-model because:
- It balances the predictions well.
- It prevents overfitting with regularization.
- It works well with correlated inputs (which stacked predictions often are).

### **Step 4: Evaluate Performance**  
I compared the **Root Mean Squared Error (RMSE)** for:
- Each individual model
- The stacked model

The **stacked model consistently outperformed the individual models**, proving the strength of combining multiple learners.

---

## Results
| Model | RMSE (Lower is Better) |
|--------|----------------|
| Linear Regression | 27878.786 |
| Random Forest | 25708.871 |
| XGBoost | 23123.952 |
| LightGBM | 23398.139 |
| GradientBoost | 23123.952 |
| **Stacked Model** | **21906.231** |

While exact values depend on data splits and hyperparameters, **the stacked model outperformed all individual models**.

---

## Why Stacking Works
- **Uses multiple perspectives** – Linear models are good for interpretability, while tree-based models handle non-linearity. Stacking blends them.  
- **Reduces bias & variance** – A single model may overfit or underfit, but stacking balances these issues.  
- **Learns optimal weights automatically** – Unlike bagging/boosting, stacking lets the meta-model decide which base models matter most.  

---

## Notes 
This project is mainly focused on showcasing **how stacking works**. The model can be improved by:
- **Better feature engineering** – Creating new features to improve predictions.  
- **Hyperparameter tuning** – Using **Optuna** to find the best settings.  
- **Using more powerful base models** – Incorporating advanced models in the ensemble.  

---
