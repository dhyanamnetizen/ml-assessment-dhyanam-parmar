##B1. Problem Formulation

(a) Problem Definition
    To optimize retail performance, we must define this as a Supervised Learning (Regression) task.

    Target Variable: Total Items Sold (Sales Volume) per store per month.

    Input Features: * Store Features: Size, Location Type (Urban/Rural), Competition Density.

    Customer/Market: Monthly Footfall, Demographics.

    Temporal: Month, Festival flags (e.g., Diwali/Christmas), Weekend count.

    Actionable Feature: Promotion Type (the "levers" we can pull).

    Justification: We are predicting a continuous numerical value (quantity) to manage inventory and staffing, making Regression the appropriate choice.

(b) Target Variable Selection: Volume vs. Revenue
    While revenue is the "bottom line," Items Sold is a superior target for measuring promotion effectiveness. Revenue can be skewed by price fluctuations or          luxury vs. budget item mixes. Sales volume is a direct reflection of customer demand and price elasticity. In real-world ML projects, predicting volume            ensures the supply chain is optimized—revenue doesn't tell you how many boxes to ship, but "Items Sold" does.

(c) Alternative Modeling Strategy
    A single global model often misses local nuances. I propose a Clustered Modeling approach. By using Unsupervised Learning (like our K-Means results in Part        A), we can group stores into "Personas" (e.g., High-Footfall Urban vs. Low-Footfall Rural). Training a separate model for each cluster allows the machine to       learn that a "Loyalty Bonus" might work for urban VIPs while a "Flat Discount" is necessary for rural, price-sensitive segments.

##B2. Data and EDA Strategy

    (a) Data Consolidation
        The final modeling dataset would be joined on Store_ID and Transaction_Month.

        Grain: One row per Store per Month.

        Aggregations: Transactions would be summed into Total_Items, while footfall would be averaged across the month to create a stable predictor.

    (b) EDA Process
        Feature Correlation Heatmap: To identify which environmental factors (like competition) are most heavily linked to sales.

        Promotion Boxplots: To visualize which of the five promotion types shows the highest median sales and lowest variance.

        Footfall vs. Sales Scatter Plot: Colored by location type to identify the "conversion rate" of different store formats.

        Seasonal Line Charts: To separate "Natural Growth" (holidays) from "Promotion-Driven Growth."

    (c) Addressing Class Imbalance
        If 80% of data has no promotion, the model will become "lazy" and biased toward baseline sales.

        Strategy: I would use Stratified Sampling to ensure the model sees enough "Promotion-Active" months. Additionally, I would use Residual Analysis to see            where      the model fails most—often in the minority class (promotion periods)—and adjust feature weights accordingly.

##B3. Model Evaluation and Deployment
      (a) Evaluation Setup
          A random train-test split is inappropriate here due to the temporal nature of retail. We must use a Time-Series Split (e.g., training on 2024-2025 and             testing on 2026). Random splits cause "Data Leakage" where the model uses future festival knowledge to predict past sales.

          Metrics: I would prioritize MAE (Mean Absolute Error) because it tells stakeholders exactly how many "units" the prediction is off by in plain English.

    (b) Interpreting Recommendations
        Using Feature Importance explains why recommendations change. For Store 12, the model might suggest "Loyalty Points" in December because is_festival is             the        dominant feature then. However, in March (a slow month), the model sees that price_sensitivity rises, prompting it to recommend a "Flat                 Discount" to kickstart     stagnant footfall.

    (c) Deployment and Monitoring
        Deployment: The model would be saved as a serialized object (.joblib) and served via a monthly batch-prediction pipeline.
