# Technical Notes

## Data Cleaning

Had to convert Yes/No columns to 1/0 because the model only understands numbers.

TotalCharges column was mixed (some strings, some numbers). Fixed by converting everything to numeric and filling NaN with 0.

Dropped customerID - it's just an identifier, doesn't help predict anything.

## Feature Engineering

Created 5 new features:

1. **IsMonthToMonth** - Flag if customer has month-to-month contract
   - These customers can leave anytime (no lock-in)
   - Had the biggest impact on churn rate

2. **IsNewCustomer** - Flag if tenure < 6 months
   - First 6 months are critical for retention
   - New customers have ~40% churn rate, older customers ~10%

3. **HighCharge** - Flag if monthly charge > median
   - Higher charges = more price-sensitive customers
   - Easier targets for competitors

4. **AvgMonthlySpend** - Monthly charges divided by tenure
   - Captures how much value customer is getting
   - High value = stickier customer

5. **TotalToMonthlyRatio** - Total charges divided by monthly charge
   - Simple proxy for tenure without using tenure directly
   - Helps the model understand commitment level

## Model Choice

Tried logistic regression. Why?
- Simple and interpretable (can explain predictions)
- Fast to train and deploy
- Probability output makes sense for business (0-100% risk)
- Works well with this dataset size

Considered random forest but decided against it:
- Too much of a black box (can't explain why someone is flagged)
- Slower to deploy
- Probably would get 1-2% more accuracy but not worth it

## Train-Test Split

80/20 split (5,634 train, 1,409 test).

Used stratification to keep the 26.5% churn rate in both sets. Without it, test set might randomly end up with 20% or 35% churn, making evaluation metrics meaningless.

## Results Interpretation

**Accuracy: 79.5%**
Correctly predicted 79.5% of customers. Not bad, but with 73.5% non-churners, a dummy model predicting "no churn" for everyone would get 73.5%. So real accuracy is ~6% above baseline.

**Precision: 65%**
Of 218 customers we flagged as high-risk, 142 actually churned. That means 76 false positives (wasted retention offers).

**Recall: 55%**
Found 232 out of 423 actual churners. Missed 191. Not great, but acceptable for business use.

**F1: 60%**
Balanced metric between precision and recall.

**ROC-AUC: 0.83**
Model ranks a random churner higher than a random non-churner 83% of the time. Random guessing = 0.50, perfect = 1.0. So 0.83 is solid.

## Issues & Decisions

**Class imbalance (26.5% churners):**
Handled by stratified split + using multiple metrics. Didn't oversample or underweight because the imbalance is moderate and stratification handles it.

**Feature correlation:**
Tenure is highly correlated with total charges (older customers spent more). But both have different impacts on churn, so kept both.

**Point-in-time data:**
This is a snapshot, not a time series. Doesn't capture trends (is churn risk increasing or decreasing?). Would need monthly data to build a better model.

**Causation vs correlation:**
Month-to-month contracts correlate with churn, but we can't say contracts CAUSE churn. Could be that risky customers choose month-to-month. Would need controlled experiments to validate.

## Monitoring

Should retrain monthly. If accuracy drops below 75%, investigate why (churn rate changed? customer composition changed? model degraded?).

Also track: are the customers we flag as high-risk actually churning? If precision drops below 50%, model is useless.

## Limitations

- Missing data: Don't have customer support interaction history, complaint data, network quality metrics
- Time: This is a snapshot; can't capture seasonal patterns or trends
- Causation: Can identify correlations but not root causes
- Validation: Haven't A/B tested whether retention offers actually work on flagged customers

## What Worked Well

Feature engineering approach. Instead of using raw features, business-informed features (IsMonthToMonth, IsNewCustomer) made the model more interpretable and probably more accurate.

Stratified split. Without it, evaluation would be on non-representative test set.

## What I'd Do Differently

- Collect more features (support interactions, network quality, competitor mentions in customer feedback)
- A/B test retention offers to validate ROI
- Time series model instead of snapshot (track churn risk over time)
- Root cause analysis (why exactly does month-to-month contract cause churn? is it the contract itself or the type of customer who picks it?)
