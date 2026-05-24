# Customer-Churn-Prediction-Analysis

Predictive analytics project focused on telecom customer churn using Logistic Regression, feature engineering, and business-driven evaluation metrics. Includes churn risk scoring and Power BI-ready outputs for retention analysis.

## Objective

Identify customers with high churn risk using historical telecom customer data and generate actionable insights to support customer retention efforts.

## Dataset

IBM Telco Customer Churn dataset sourced from Kaggle.

Dataset size: 7,043 customers
Target variable: Churn (Yes/No)

Data includes:
Customer demographics,
Service subscriptions,
Contract information,
Billing and payment details,
Customer tenure,
Churn status

## Workflow
- Data Preparation
- Cleaned and validated billing-related fields
- Converted categorical variables into machine-readable format
- Removed irrelevant identifiers

 
 ## Feature Engineering
 
Created business-oriented features such as:
- New customer indicator
- High monthly charge indicator
- Month-to-month contract flag
- Spending and tenure-based ratios

Modeling
- Logistic Regression classifier
- Stratified train-test split
- 80/20 training and testing setup
- Evaluation

## Evaluated model performance using:

Accuracy,
Precision,
Recall,
F1 Score,
ROC-AUC,
Reporting

## Generated:

- Churn probability predictions
- Customer risk segmentation
- Feature importance analysis
- Power BI-ready datasets

## Model Performance
- Metric	Score
- Accuracy	79%
- Precision	65%
- Recall	55%
- F1 Score	59%
- ROC-AUC	0.83

## Interpretation
- 65% of customers flagged as high-risk actually churned
- The model identified roughly half of actual churners
- ROC-AUC of 0.83 indicates strong class separation capability

## Key Insights
- Month-to-month contracts showed significantly higher churn risk
- Short-tenure customers were more likely to churn
- Higher monthly charges correlated with increased churn probability
- Customers without tech support or security services showed higher churn tendency
- Contract structure and tenure emerged as strong predictive signals

## Business Use Cases
- Customer retention prioritization
- Risk-based customer segmentation
- Retention campaign targeting
- Churn monitoring dashboards
- Predictive support workflows

The model also categorizes customers into:
- Low Risk
- Medium Risk
- High Risk
based on predicted churn probability.

## Project Files
- File	Description
- Telco_Churn_Prediction.ipynb	Complete analysis and modeling notebook
- feature_importance.csv	Logistic Regression coefficients
- churn_predictions.csv	Prediction results on test data
- churn_predictions_for_powerbi.csv	Processed dataset for Power BI
- churn_dashboard.pbix	Power BI dashboard

## Tech Stack
Python, Pandas, NumPy, Scikit-learn, Matplotlib, Seaborn, Power BI, Google Colab

## Limitations
- Dataset represents a static snapshot of customer behavior
- Additional behavioral or temporal features may improve prediction quality
- Correlation does not imply causation
- Production deployment would require continuous retraining and monitoring

## Future Improvements
- Experiment with ensemble models such as Random Forest or XGBoost
- Incorporate time-series or behavioral interaction features
- Perform retention campaign A/B testing
- Deploy as an interactive dashboard or API service

End-to-end churn analytics project combining data preprocessing, feature engineering, predictive modeling, evaluation, and business interpretation.
