🔍 1. Project Overview

Cloud services give companies massive flexibility — but also unpredictable and rapidly growing costs.
AWS billing often fluctuates due to:

scaling events

new deployments

data transfer spikes

changing usage patterns

unexpected resource inefficiencies

This project builds a time-series forecasting system to predict future AWS monthly costs and provide actionable insights for cost optimization.

The model forecasts:

Expected cost for next N months

Confidence intervals for budgeting

Anomaly detection for unusual spending

Trend + seasonality decomposition

This project demonstrates:

Advanced time-series modeling

Forecasting evaluation (MAPE, RMSE, MASE)

Trend/seasonality decomposition

Feature enrichment with cloud metadata

Model comparison across ARIMA, Prophet, XGBoost/LGBM regressors

📂 2. Dataset

Source: AWS Cost Explorer (or synthetic dataset if using sample data)
Frequency: Monthly or daily billing data
Columns typically include:

date

TotalCost

Optional breakdowns:

EC2Cost, S3Cost, LambdaCost, DataTransferCost

UsageType, Service

⚠️ Forecasting challenges:

Cloud cost is often non-stationary

Sudden spikes due to deploy events

Weekly/monthly seasonal patterns

Costs may be partly controlled by engineering behavior changes

Need robust models that do not overfit spikes

🧹 3. Data Preprocessing
✔ Missing values

Forward-filled or imputed using monthly medians

Rare for billing data, but validated

✔ Timestamp parsing

Converted to datetime index

Resampled to monthly totals (if dataset was daily)

✔ Outlier detection

Spikes identified using:

Z-score thresholding

IQR-based filtering

Rolling-median deviation

Optionally, outliers can be:

Winsorized (capped)

Modeled separately as anomaly signals

Included as regressors

✔ Feature engineering

Added:

Month number (1–12)

Year

Day-of-week (if using daily data)

Lag features:

t-1, t-2, t-3 months

Rolling averages:

3, 6, 12-month rolling means

Rolling cost volatility (useful for XGBoost models)

🧠 4. Forecasting Models

Multiple forecasting models were trained to compare performance.

(1) ARIMA / SARIMA

Captures linear trends + seasonal patterns

Good for stable, predictable usage

Struggles with nonlinear spikes

(2) Facebook Prophet

Handles trend & seasonality automatically

Good for business-style time series

Naturally handles outliers, holidays, and changepoints

(3) LightGBM / XGBoost Regressor

Using engineered lag features, these models capture:

Nonlinear spend relationships

Spikes and irregular patterns

Multi-service usage interactions

(4) Baseline models

Naive forecast (last value)

Seasonal naive (last year’s month)

These baselines are required for proper benchmarking.

📊 5. Evaluation Metrics

Cloud cost forecasting is evaluated using:

Primary Metrics

MAPE – percentage error (easy to interpret)

RMSE – penalizes large spikes

MAE – robust to outliers

MASE – scale-independent benchmark

Secondary Metrics

Coverage of prediction intervals

Directional accuracy (correctly predicting cost going up/down)

Typical expected performance:

MAPE < 15% is strong

MASE < 1 beats naive models

Prophet or LightGBM often outperform ARIMA on cloud data

🏆 6. Results Summary

Replace placeholders with your actual results.

Example (typical results using AWS billing data):
Model	MAPE	RMSE	Best Use Case
ARIMA	18%	Moderate	Stable workloads
Prophet	12%	Low	Seasonal + trending costs
LightGBM	9%	Lowest	Irregular, nonlinear usage
Naive baseline	22%	High	For comparison only

Best model: LightGBM Regressor
Forecast horizon: 3–6 months
Insight: Identified a recurring cost spike every Q4 due to storage increases

📈 7. Visualizations

Your notebook includes:

Actual vs Forecasted cost line plot

Forecast residual plots

Prophet decomposition (trend + weekly + monthly seasonality)

Rolling window error metrics

Anomaly detection chart

Feature importance for gradient boosting models

These visuals are extremely valuable for cost optimization teams.

🧠 8. Business Interpretation & Cost Optimization Insights

This model can be used for:

✔ Budgeting & Financial Planning

Forecast Q1–Q4 spend

Compare expected vs. actual spend

✔ Detecting anomalies

If predicted cost = $42k but actual = $61k → instant red flag.

✔ Identifying optimization opportunities

Analysis shows:

Which services drive cost increases

When usage spikes occur

Seasonal cost patterns

Resource categories with high variability

✔ Right-sizing & resource planning

Example insights:

EC2 cost spikes every end-of-quarter → possible scaling inefficiency

S3 spending shows linear upward trend → retention policy review needed

This connects ML to real business value.

🛠 9. How to Run
Clone the repo
git clone https://github.com/DemonCoder70/aws-cost-forecasting.git
cd aws-cost-forecasting

Install dependencies
pip install -r requirements.txt

Run the notebook
jupyter notebook aws-cost-forecasting.ipynb

🚀 10. Future Improvements

Add multi-service forecasting (EC2/S3/Lambda separately)

Build a Streamlit dashboard for interactive forecasting

Add anomaly alerts using statistical process control

Use NeuralProphet or LSTM for more complex series

Integrate AWS Cost Explorer API for automated daily updates

Build a FinOps-friendly cost breakdown explorer
