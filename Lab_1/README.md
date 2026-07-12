# MSCS_634_Lab_1: Data Exploration, Visualization, and Preprocessing

## Overview and Purpose
The purpose of this lab is to practically apply data mining fundamentals—specifically data visualization, preprocessing, and statistical analysis using a Jupyter Notebook environment. By utilizing Python libraries such as Pandas, Matplotlib, and Seaborn, this lab transforms raw retail sales data (`retail_sales_dataset.csv`) into a clean, structured format suitable for deeper analytical modeling. The dataset contains a mixture of numerical features (sales amount, profit, age) and categorical features (product category, region, payment method).

## Key Insights from Visualizations and Statistical Measures
*   **Visual Discoveries:** 
    *   The **Line Plot** tracking monthly sales revealed temporal trends in purchasing behavior, highlighting specific peak revenue months. 
    *   The **Box Plot** of profit spread by region demonstrated that while average profit margins remain relatively consistent geographically, certain regions possess a higher density of high-yield outliers.
    *   The **Pie Chart** of payment methods provided a clear categorical breakdown, showing the dominance of specific transaction types (e.g., Debit Card vs. EMI) among the customer base.
*   **Statistical Findings:** 
    *   The central tendency measures (mean vs. median) for `sales_amount` indicated a right-skewed distribution, meaning a smaller number of high-value purchases pulled the average up above the median.
    *   The correlation matrix highlighted the mathematical relationships between numerical features, confirming the expected positive correlation between `quantity`, `sales_amount`, and overall `profit`, while showing how `discount_pct` inversely impacts final profit margins.

## Challenges Faced and Decisions Made
1.  **Handling Corrupted Temporal Data:** A significant challenge was discovered during the date-parsing phase. When converting the `order_date` column to datetime objects, 273 records contained invalid or corrupted date strings. The decision was made to use the `errors='coerce'` parameter to safely convert these to `NaT` (Not a Time) missing values, and subsequently drop those specific rows to ensure accurate time-series charting.
2.  **Outlier Management:** When scanning for outliers in the `sales_amount` column, the Interquartile Range (IQR) method identified a subset of exceptionally high-value transactions. The decision was made to filter out values falling beyond the 1.5 * IQR upper boundary to prevent these "whales" from skewing the standard statistical distribution during this foundational analysis.
3.  **Data Discretization:** For demographic analysis, continuous `age` data was too granular to spot broad trends. A decision was made to discretize this data using equal-width binning, categorizing customers into 'Youth', 'Adult', and 'Senior' brackets to make categorical comparisons easier.