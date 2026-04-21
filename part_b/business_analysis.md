# Business Case Analysis

---

# B1. Problem Formulation

### (a) Machine Learning Problem

This can be formulated as a **supervised regression problem**.

* **Target Variable:**
  `items_sold`

* **Input Features:**
  store_id, store_size, location_type, promotion_type,
  is_weekend, is_festival, competition_density,
  temporal features (month, day_of_week)

* **Problem Type:**
  Regression, because the goal is to predict a **continuous numeric value (items sold)**.

---

### (b) Why items_sold is better than revenue

Using **items_sold** is more reliable than revenue because:

* Revenue can vary due to pricing, discounts, or product mix
* Items sold reflects actual **customer demand and promotion effectiveness**
* It avoids distortion caused by pricing strategies

**Broader Principle:**
The target variable should directly represent the **business objective** and not be influenced by external or confounding factors.

---

### (c) Alternative Modeling Strategy

Instead of a single global model:

* Use **segmented models** (e.g., separate models for urban, semi-urban, rural stores)
* Or include **interaction features** (store × promotion)

**Justification:**
Different stores behave differently, so a single model may not capture these variations effectively.

---

# B2. Data and EDA Strategy

### (a) Data Joining and Granularity

* Join tables using:

  * `store_id` → store attributes
  * `promotion_type` → promotion details
  * `transaction_date` → calendar data

* **Final dataset grain:**
  One row = **store × day (or store × promotion period)**

* **Aggregations:**

  * Total items sold per store per day/month
  * Average basket size
  * Promotion frequency
  * Customer visit trends

---

### (b) EDA Analysis

1. **Distribution of items_sold**

   * Check skewness and outliers
   * Helps decide transformations

2. **Promotion vs Sales**

   * Compare average sales across promotion types
   * Identify most effective promotions

3. **Time-based trends**

   * Monthly / weekly trends
   * Identify seasonality

4. **Correlation heatmap**

   * Identify important features
   * Detect multicollinearity

👉 These insights help in:

* Feature selection
* Creating new features
* Model improvement

---

### (c) Imbalance Issue (80% no promotion)

* Model may become biased toward “no promotion” cases
* Poor learning for promotion effectiveness

**Solutions:**

* Resampling (oversample promotion cases)
* Use weighted models
* Stratified evaluation
* Separate modeling for promotion vs non-promotion

---

# B3. Model Evaluation and Deployment

### (a) Train-Test Strategy

* Use **time-based split** (train on past, test on future)

**Why not random split?**

* It causes **data leakage**
* Future data may influence training

**Evaluation Metrics:**

* RMSE → penalizes large errors
* MAE → interpretable average error

---

### (b) Explaining Model Recommendations

Use **feature importance**:

* Identify which features influenced predictions
* Example:

  * December → festivals → high sales → loyalty bonus works
  * March → low demand → discounts work better

**Communication:**

* Explain insights in simple business terms
* Show charts or feature importance plots

---

### (c) Deployment Strategy

1. **Model Saving:**

   * Save using pickle or joblib

2. **Monthly Pipeline:**

   * Collect new data
   * Apply same preprocessing
   * Generate predictions

3. **Monitoring:**

   * Track RMSE / MAE over time
   * Detect performance drop

4. **Retraining:**

   * Retrain when performance degrades
   * Or periodically (e.g., monthly/quarterly)

---

# ✅ Conclusion

This approach ensures:

* Accurate prediction of sales
* Better promotion strategy
* Scalable and production-ready ML system
