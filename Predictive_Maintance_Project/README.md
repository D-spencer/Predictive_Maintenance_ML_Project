# Machine Maintenance Failure Prediction

## Project Backgrounds

This project involves building a machine learning system that predicts machine failures using sensor and operational data. Predictive Maintenance (PdM) enables industries to detect early warning signs, reduce downtime, and perform maintenance only when necessary.

## Project Objective

To deliver a reliable classification model that can identify equipment at high risk of failure. This will enable the maintenance team to execute proactive and targeted interventions, thereby reducing unplanned downtime and minimizing the high costs associated with unexpected breakdowns.
## Data Source
The project used a publicly available Predictive Maintenance Dataset, sourced from Kaggle[https://www.kaggle.com/datasets/shivamb/machine-predictive-maintenance-classification/data']
The dataset contains 10,000 machine records across 14 features. These features capture key operational conditions such as sensor measurements, machine load, and failure types, which are essential for developing the classification model.

## Data Inspection and Cleaning

Initial inspection for data type mismatch, missing values, duplicates, and incorrect entries revealed no significant data quality issues.

## Key Insights from Exploratory Data Analysis (EDA)

* Confirmed the target variable was imbalanced, with a failure rate of approximately 3% (97% no failure, 3% failed). This necessitated the use of recall as the primary evaluation metric.

* Failure appears to be notably more common among low machine type.

* heat dissipation seems to be the most cause of failure 

## Key Insights from Cluster Analysis

K-Means was used to identify 4 distinct failure segments that informed feature engineering. The following are observations from the clustering results:

* The machine were mainly separated by machine type. Specifically, all machine in Cluster 2 are from medium, all machine in Cluster 4 are from high, and all machine in Clusters 1 and 3 are from low.


## Model Development

### Splitting Method and Evaluation Metrics

The data was split into training and testing sets using a stratified sampling method to preserve the class distribution in the target variable.

Given the class imbalance (3% failure), Accuracy was deemed insufficient. The evaluation metrics used were majorly precision and recall, however recall was used for model selection and tuning. Maximizing recall was essential to minimize False Negatives (i.e., failing to identify a machine that will fail), which directly aligns with the business objective of successful retention campaigns.

### Model Training and Selection

Four models were initially trained: Logistic Regression, Decision Tree, Random Forest, and Gradient Boosting.

The Gradient Boosting model demonstrated the strongest initial performance, achieving a recall of 0.720. The final model was developed through the following iterative optimization steps:

1. **Hyperparameter Tuning**: The learning rate was optimized to mitigate the risk of overfitting, resulting in a slight drop in recall (0.632).

2. **Feature Engineering**: Model performance was further enhanced by incorporating K-Means distance features, which reduced recall to 0.602.

3. **Optimal Threshold Tuning**: Given the class imbalance, the model's prediction threshold was tuned to maximize recall across both positive and negative classes. This final step yielded the deployed model with a performance of 0.882 Recall and 0.257 Precision on the test set.

## Business Impact Assessment

To measure the model’s practical value, its projected cost savings were compared against a naive baseline that assumes no failures and triggers no maintenance.

For this assessment, the following cost assumptions were used:

* Unplanned breakdown cost: €5,000 per machine

* Scheduled preventive maintenance cost: €800 per machine

Based on the test-set confusion matrix, the model correctly identified 60 potential failures, preventing expensive breakdowns and allowing the maintenance team to act early. Although it triggered 173 false alarms, these incur a significantly lower cost compared to unplanned downtime.
When the costs of true positives, false positives, and residual failures (false negatives) are evaluated together, the predictive maintenance model is projected to reduce overall maintenance-related expenses by roughly 40–55% compared to operating without any predictive system.

## Next Steps

The following steps are recommended to move the project toward full production and sustained impact:

* Collaborate with ML Engineers for model deployment into the production environment.

* Perform hyperparameter tunning to identify the most optimal number of clusters for the K-Means preprocessing step.

* Perform hyperparameter search to find more optimal tree and ensemble parameters for the Gradient Boosting model.

* Try other models such as K-Nearest Neighbours and Support Vector Machines.