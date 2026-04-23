# Part B: Business Case Analysis

# Scenario: Promotion Effectiveness at a Fashion Retail Chain

# B1. Problem Formulation

# B1(a) Machine Learning Problem Definition

The objective is to predict the number of items sold for a given store, time, and promotion.

- Target Variable: "items_sold"

- Input Features: "store_id", "store_size", "location_type", "promotion_type", "is_weekend", "is_festival", "competition_density", and date-based features such as "month" and "day_of_week"

- Type of Problem: Supervised Learning – Regression

This is a regression problem because the target variable ("items_sold") is a continuous numerical value.

# B1(b) Why items_sold instead of revenue

Using items_sold is more reliable than revenue because revenue can be influenced by pricing strategies, discounts, and product mix, which may not directly reflect customer demand.

Items sold provides a clearer and more direct measure of promotion effectiveness in driving sales volume.

Broader Principle:
The target variable should closely align with the business objective and should avoid being influenced by external or confounding factors.

# B1(c) Alternative Modeling Strategy

Using a single global model across all stores may not capture differences in customer behavior.

A better approach is:

- Build segmented models (e.g., separate models for urban, semi-urban, and rural stores), or
- Include store-specific features or interaction terms

This approach captures variations in customer preferences and improves prediction accuracy.

# B2. Data and EDA Strategy

# B2(a) Data Integration and Dataset Design

The raw data comes from multiple tables:

- Transactions
- Store attributes
- Promotion details
- Calendar data

These tables can be joined as follows:

- Join transactions with store attributes using "store_id"
- Join promotion details using "promotion_type"
- Join calendar data using "transaction_date"

Final Dataset Grain:
Each row represents one store on one date (store-date level)

Aggregations:

- Total "items_sold" per store per day
- Average basket size
- Promotion usage indicators

# B2(b) EDA Strategy

1. Sales Distribution Analysis
   
   - Plot histogram of "items_sold"
   - Identify skewness and outliers

2. Promotion Effectiveness
   
   - Compare average sales across promotion types
   - Identify which promotions perform best

3. Time-based Trends
   
   - Analyze sales by month and day of week
   - Capture seasonality and weekly patterns

4. Store-Level Analysis
   
   - Compare performance across store sizes and locations
   - Identify regional differences

Impact on Modeling:

- Helps in feature engineering (seasonality, interactions)
- Guides feature selection and transformations

# B2(c) Handling Imbalance (80% No Promotion)

Since 80% of transactions occur without promotions, the model may become biased toward non-promotional scenarios.

Impact:

- Model may fail to learn the true effect of promotions
- Reduced prediction accuracy for promotional cases

Steps to Address:

- Use sampling techniques (oversample promotion cases)
- Create interaction features (promotion × store type)
- Evaluate performance separately for promotion vs non-promotion

# B3. Model Evaluation and Deployment

# B3(a) Train-Test Split and Metrics

Since the data is time-based, a temporal split should be used:

- Train on earlier data
- Test on the most recent data

A random split is inappropriate because it can lead to data leakage, where future data influences training.

Evaluation Metrics:

- RMSE (Root Mean Squared Error): Penalizes large errors
- MAE (Mean Absolute Error): Measures average prediction error

Interpretation:
Lower RMSE and MAE indicate better model performance and more accurate predictions.

# B3(b) Explaining Different Recommendations

The model may recommend different promotions for the same store at different times due to changes in features.

For example:

- December may have festivals → higher demand → loyalty-based promotions perform better
- March may have lower demand → discount-based promotions are more effective

Feature importance analysis shows that variables such as "month", "festival", and "promotion_type" influence predictions.

This helps explain model decisions to the marketing team.

# B3(c) Deployment Strategy

1. Model Saving
   
   - Save the trained model using "joblib" or "pickle"

2. Data Pipeline
   
   - New monthly data is processed using the same preprocessing pipeline

3. Prediction
   
   - The model generates predictions for each store at the start of the month

4. Monitoring
   
   - Track model performance (RMSE, MAE) over time
   - Monitor data drift and feature distribution changes

5. Retraining
   
   - Retrain the model periodically (e.g., monthly or quarterly) when performance declines

# Final Conclusion

The proposed machine learning solution enables data-driven decision-making for promotion strategies across stores. By leveraging historical data, feature engineering, and robust modeling, the business can optimize promotions to maximize sales and improve overall efficiency. 