# ML Workflow Assignment

## Task 1 — Label and Data Leakage

### Label Column:
**repeat_purchase_flag**

Justification: This column indicates whether a customer made a repeat
purchase within 30 days (1 = yes, 0 = no). This is the target variable
that the model is trying to predict.

### Data Leakage Column:
**discount_used_on_repeat_order**

Justification: This column contains information about a discount applied
during the repeat purchase. It is only available after the repeat
purchase has occurred, meaning it reveals the outcome in advance.
Using it as a feature would introduce data leakage.

---

## Task 2 — Steps Before Training a Model

### Step 1: Exploratory Data Analysis (EDA)
EDA helps in understanding the dataset by identifying missing values,
outliers, and patterns in the data. It ensures that the data is clean
and meaningful before training the model.

### Step 2: Data Preprocessing and Feature Engineering
This step involves cleaning the data, handling missing values, removing
leakage features, and preparing the data for model training. Proper
preprocessing improves model accuracy and reliability.

---

## Final Note
Following these steps ensures:
- No data leakage
- Clean and reliable data
- Better model performance
