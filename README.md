# Accredian - Instructor Effectiveness Analysis (Report)

This repository contains a Jupyter notebook (Accredian_Assignment.ipynb) that performs an end-to-end analysis to measure and predict instructor effectiveness using course / batch-level metrics. This README is a concise report summarizing the notebook, reproducing the analysis overview, and showing how to reproduce the final evaluation plots.

I read through the notebook and extracted the analysis flow (EDA → define effectiveness → aggregate → modeling → evaluation). Below I summarize the dataset, the main steps performed, the cleaning decisions, and instructions to reproduce the evaluation plots (links to view the plots inside the notebook are included).

---

## Table of contents
- Overview
- Dataset
- Exploratory Data Analysis (EDA) — key observations
- Data cleaning & outlier removal
- Modeling approach (summary)
- Final evaluation plots (how to view / reproduce)
- How to run
- Notes & next steps

---

## Overview
Goal: Create an Instructor Effectiveness Score and build a model to predict instructor effectiveness tier. The notebook performs:
- EDA of batch-level teaching metrics
- Definition and aggregation from batch → instructor
- Outlier detection & removal
- Exploratory visualization (boxplots, histograms, pairplots)
- Model building to predict an instructor effectiveness tier and evaluation of results

The original notebook: Accredian_Assignment.ipynb
View it on GitHub: https://github.com/ShubhamP1028/Accredian-DS-Assignment/blob/b3a6909a558a0131ecf47e5c04da435d785dec35/Accredian_Assignment.ipynb

---

## Dataset
Filename used in the notebook:
- `instructor_effectiveness_dataset_2000_rows.xlsx`

Columns (as seen in the notebook):
- batch_id (object)
- instructor_id (object)
- course_id (object)
- completion_rate (float)
- avg_score_improvement (float)
- avg_quiz_score (float)
- dropout_rate (float)
- avg_watch_time (float)
- assignment_submission_rate (float)
- forum_activity_rate (float)
- avg_feedback_score (float)
- feedback_response_rate (float)

Sample: The notebook reads the file into `df`:
- `df = pd.read_excel('instructor_effectiveness_dataset_2000_rows.xlsx')`

Basic dataset shape and stats (from notebook outputs)
- Initial rows: 2000
- Numeric column means and ranges (excerpt):
  - completion_rate: mean ≈ 0.603 (min 0.30, max 0.98)
  - avg_score_improvement: mean ≈ 27.04 (min ≈ 6.16, max 40)
  - avg_quiz_score: mean ≈ 77.96 (min ≈ 40.39, max 100)
  - avg_feedback_score: mean ≈ 4.21 (min ≈ 2.64, max 5)
- No nulls and 0 duplicate rows (noted in the notebook)

---

## Exploratory Data Analysis (EDA) — key observations
- Distribution summaries shown using:
  - `df.describe()`
  - boxplots for numeric columns
  - histograms for numeric features
  - pairplot (sampled) to inspect pairwise relationships
- Several numeric columns show potential outliers (examined via boxplots):
  - `avg_score_improvement`, `avg_quiz_score`, `avg_feedback_score`

---

## Data cleaning & outlier removal
- Outliers were removed using IQR method for the three columns above.
- Removal summary (noted in the notebook):
  - Initial DataFrame shape: (2000, 12)
  - Removed 6 outliers from `avg_score_improvement`
  - Removed 11 outliers from `avg_quiz_score`
  - Removed 11 outliers from `avg_feedback_score`
  - Final shape after outlier removal: (1972, 12)

After cleaning, the notebook re-plots boxplots, histograms and pairplot to confirm distributions and relationships.

---

## Aggregation & Instructor Effectiveness Score
The notebook states the analysis tasks as:
- Define an Instructor Effectiveness Score
- Aggregate batch → instructor

(Details and exact formula for the Instructor Effectiveness Score are implemented in the notebook. For the precise scoring formula and the aggregation code, please consult the notebook above — the score construction and weights are shown inline there.)

---

## Modeling approach (summary)
- Task: Predict effectiveness tier (a discrete tier label for instructors) using aggregated instructor features.
- Typical steps in the notebook:
  - Aggregate batch metrics to instructor-level features
  - Create target tier (e.g., binning effectiveness score into tiers)
  - Train/test split
  - Train supervised classifiers (commonly used: Logistic Regression, Random Forest, XGBoost)
  - Evaluate using confusion matrix, classification report, accuracy / F1 / precision / recall, and (if applicable) ROC curves

The notebook includes training and final evaluation cells — see the notebook for exact models tried and metric values.

---

## Final evaluation plots (view & reproduce)
I could not extract the rendered images from the notebook into separate files via the GitHub API in this session, but the notebook includes the final evaluation plots (confusion matrix, ROC curve(s), feature importance and other visualizations). You can view them directly by opening the notebook file on GitHub (renders the figures in-place) or run the notebook locally to regenerate and save each plot.

Direct link to view the notebook (rendered with outputs on GitHub):
- https://github.com/ShubhamP1028/Accredian-DS-Assignment/blob/b3a6909a558a0131ecf47e5c04da435d785dec35/Accredian_Assignment.ipynb

- Linear Regression Actual vs Predicted plot -
  <img width="833" height="547" alt="image" src="https://github.com/user-attachments/assets/33d80d38-cf43-4516-af8c-d6ec39c7f6f0" />

- Logistic Regression Confusion matrix plot:
  <img width="649" height="547" alt="image" src="https://github.com/user-attachments/assets/309fcf1b-ff1d-4417-a466-576f4651732b" />
