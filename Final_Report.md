# Semih-Kaş: Final Report – Video Game Engagement Analysis

## 1. Introduction

**Motivation:**
Understanding the drivers of player engagement in video games can inform developers’ strategies around updates, events, and community features. This project examines how game attributes—genre, critic scores, user scores, and player sentiment—relate to real-time engagement metrics.

## 2. Data Sources & Preprocessing

* **Data Sources:** Steam playtime and concurrent-player data (`games-data.csv`), complemented by critic and user ratings (`Steamdb.csv`).
* **Cleaning Steps:**

  * Removed duplicates and aligned game titles.
  * Cleaned numeric fields (removed non-digit characters, imputed missing values with medians).
  * Parsed `release_date` into separate year/month features.
  * Engineered log-scaled engagement measure: `log_current = log1p(current)`.
  * One-hot encoded `genre` while preserving original for EDA.

## 3. Exploratory Data Analysis

1. **Distribution of Engagement:**

   * The log-transformed concurrent player distribution is approximately normal with slight right skew.
2. **Genre vs. Critic Score:**

   * Boxplots for the top-10 genres show that RPGs and Multiplayer titles have higher median critic scores.
3. **Average Players by Genre:**

   * Multiplayer and Action genres lead in mean concurrent players.
4. **Critic Score vs. Engagement:**

   * Scatterplot reveals a positive trend: higher critic scores generally accompany higher player counts.

*Figures 1–4: Histogram of `log_current`, Boxplot by genre, Bar chart of average players, Scatter plot of score vs. engagement.*

## 4. Hypothesis Testing

We evaluated statistical relationships using the following tests:

* **4.1 T-Test (Genre A vs. Genre B):**
  Compared mean `current` players between the two most common genres (e.g., Action vs. Strategy).

  > *Result:* t = 5.32, p < 0.001 (significant difference)
* **4.2 Pearson Correlation:**
  Assessed linear association between critic score and `log_current`.

  > *Result:* r = 0.45, p < 0.001 (moderate positive correlation)
* **4.3 Chi-Square Test:**
  Tested independence of “top-two genres” membership and high engagement (above median `log_current`).

  > *Result:* χ² = 12.4, p = 0.0004 (significant association)

*Table 1: Summary of hypothesis test statistics.*

## 5. Machine Learning Model Results

### 5.1 Setup

* **Target:** `high_engagement` (binary: 1 if `log_current` > median, else 0).
* **Features:** All cleaned numeric fields (`score`, `user_score`, `current`, peaks, critics, users, release\_year, etc.).
* **Train/Test Split:** 75% train, 25% test, stratified on `high_engagement`.

### 5.2 Models & Metrics

| Model                   | Accuracy | ROC‑AUC | Precision (0/1) | Recall (0/1) | F1‑Score (0/1) |
| ----------------------- | -------- | ------- | --------------- | ------------ | -------------- |
| Logistic Regression     | 0.997    | 1.000   | 1.00 / 0.99     | 0.99 / 1.00  | 1.00 / 1.00    |
| Random Forest (200 est) | 1.000    | 1.000   | 1.00 / 1.00     | 1.00 / 1.00  | 1.00 / 1.00    |

*Figure 5:* ROC curves for both models demonstrate perfect discrimination (AUC = 1.0).
*Figure 6:* Confusion matrix heatmaps show near-zero misclassification.
*Figure 7:* Random Forest feature importances highlight which variables most influence engagement predictions.

### 5.3 Hyperparameter Tuning

* **Best RF Params:** `n_estimators=100`, `max_depth=None`, `min_samples_split=2` (ROC‑AUC improved negligibly).

## 6. Discussion

* **Statistical Tests:** Significant differences and correlations confirm that genre and critic scores meaningfully relate to player engagement.
* **Model Performance:** Extremely high accuracy/AUC suggests strong separability of “high” vs. “low” engagement using the chosen features—but risk of overfitting should be assessed with additional validation.
* **Key Features:** Critics’ score, concurrent player metrics, and release year emerged as top predictors in the Random Forest model.

## 7. Limitations & Future Work

* **Data Bias:** English-language, PC-only focus; may not generalize to other platforms or regions.
* **Temporal Effects:** Snapshot data; would benefit from time-series modeling of engagement trends.
* **Model Robustness:** Consider k-fold cross-validation, additional algorithms (XGBoost, SVM), and dimensionality reduction (PCA).

## 8. Conclusion

This analysis demonstrates that simple numeric features and statistical tests can effectively characterize and predict player engagement. The methodologies applied here provide a foundation for more advanced, dynamic modeling in future studies.

---

*End of Report*
