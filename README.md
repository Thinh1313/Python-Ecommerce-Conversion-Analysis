# Python Ecommerce Conversion Analysis
Session-level analysis of e-commerce user behavior to identify drivers of purchase conversion and evaluate the profitability of targeted marketing strategies using python and logistic regression

# Overview
This project analyzes user browsing sessions from an e-commerce cosmetics store to identify behavioral signals associated with purchase conversion.

Event-level browsing logs were aggregated into session-level observations capturing engagement patterns such as product views, session duration, price exposure, and cart interactions.

using these features, a logistic regression model was developed to estimate the probability that a browsing session results in a purchase. The analysis also evaluates how these predictions could support targeted marketing strategies.

# Business Question
Which user browsing sessions are most likely to convert into purchases and how can this information be used to improve marketing targeting?

# Key results

- Overall conversion rate: ~3%
- Top decile conversion rate: ~21%
- best model ROC-AUC: 0.91
- Dataset size ~1.7M events (October 2019)

# Dataset Description
**Source:** Kaggle
**Dataset:** E-commerce Events History in Cosmetics Shop
https://www.kaggle.com/datasets/mkechinov/ecommerce-events-history-in-cosmetics-shop?select=2019-Oct.csv

The dataset contains over **1.7 million user interaction logs** from an online cosmetics store for the month of October. Each row represents a single event performed by a user during a browsing session. Events include actions such as viewing products, adding items to cart, removing items from cart, and completing purchases.

Key columns include:
- `event_time`
- `event_type`
- `product_id`
- `category_code`
- `brand`
- `price`
- `user_id`
- `user_session`

Data structure:
the raw dataset contains **event-level observations**, meaning that a single browsing history may contain multiple interactions. For this project, events were aggregated into **session-level observations** to analyze behavioral patterns and build a model predicting purchase conversion.

# Feature Engineering
Event-level logs were aggregated into session-level observations to capture user engagement patterns.

Key features created include:
- `num_views` - number of product views in the session
- `session_duration` - total time between first and last event
- `num_add_cart` - number of add_to_cart events
- `avg_view_price` - average price of viewed products
- `num_unique_brand` - number of distinct brands explored

Additional transformations include:
- Log transformations to reduce skew in engagement metrics
- Cyclical encoding of time-of-day features 
- Funnel metrics such as view-to-cart and cart-to-purchase rates

# Exploratory Data Analysis
Exploratory data analysis was conducted to identify behavioral differences between converting and non-converting sessions.

Key observations:
- Converting sessions showed significantly higher engagement levels, including more product views and interactions.
- Converting sessions had substantially longer durations compared to non-converting sessions.
- User exploring a greater number of brands were more likely to complete a purchase.
- The majority of sessions never reached the cart stage, indicating significant drop-off early in the browsing funnel.

![Conversion Rate by Session Duration]("D:/projects/Python Ecommerce Conversion Analysis/Images/Python_Ecommerce_Conversion_2.png")

# Model Development
To predict whether a browsing session results in purchase, a logistic regression model was used.

Logistic regression was selected because the problem is a binary classification task and the model provides interpretable coefficients that help identify behavioral signals associated with purchase intent.

The dataset was split into training and test sets to evaluate model performance on unseen data.

Three models evaluated:
1. **Baseline Model** - basic engagement features
2. **No Cart Model** - removed cart-related features to evaluate their predictive power
3. **Feature Engineered Model** - includes additional behavioral and temporal features

# Model Evaluation
Model performance was evaluated using the **ROC-AUC metric**, which measures the model's ability to distinguish between converting and non-converting sessions.

| Model                    | ROC-AUC  |
| ------------------------ | -------  |
| Baseline Model           | 0.79     |
| No Cart Feature          | 0.72     |
| Feature Engineered Model | **0.91** | 

The feature engineered model significantly improved predictive performance, indicating that behavioral signals such as session duration and product exploration provide strong indicators of purchase intent.

# Conversion Segmentation
Sessions were ranked by predicted conversion probability and grouped into deciles. The top 10% of sessions showed a conversion rate of **21%**, compared to an overall conversion rate of roughly **3%**. This demonstrates that the model effectively identifies high-intent browsing sessions.

![Decile Conversion Chart]("D:/projects/Python Ecommerce Conversion Analysis/Images/Python_Ecommerce_Conversion_3.png")

# Marketing Simulation
To evaluate the business value of the model, a simple marketing scenario was simulated.
Sessions were ranked by predicted conversion probability, and the highest probability sessions were treated as potential targets for marketing campaigns.
Because the top decile of sessions converts at a significantly higher rate than the average session, targeting these users could improve the efficiency of advertising spend.
If a marketing campaign successfully increases conversion rates among these high-intent sessions, even a small lift in purchases could generate meaningful revenue gains.

# Business Recommendations
Based on the behavioral analysis and modeling results, several recommendations can be made:

1. **Prioritize high-intent sessions**
Users with high predicted conversion probabilities should be prioritized for marketing interventions.

2. **Encourage product exploration**
Sessions with more product views and brand exploration show strong purchase intent.

3. **Improve the cart experience**
Many sessions drop off before completing purchases, suggesting friction in later funnel stages.

4. **Implement real-time conversion scoring**
Predictive models could be used in production systems to identify high-value sessions as they occur.

# Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- Statsmodels
- Matplotlib
- Seaborn