<h1> Grab Data Analysis </h1>

## 1. Project Overview
This project analyzes Grab transaction data to detect fraud and improve data quality within the GrabUnlimited subscription program. The primary focus is to reduce significant financial loss caused by GrabUnlimited subsidy abuse from fake restaurants and to lower the high order cancellation rate that negatively impacts genuine drivers and merchants.

Key Objectives:
- Objective 1: Identify merchants suspected of being fake restaurants set up to exploit GrabUnlimited promo subsidies.
- Objective 2: Estimate the financial loss from promo subsidies wasted on fraudulent transactions.
- Objective 3: Clean and standardize inconsistent data (order status, cancellation reasons, missing values) to support reliable fraud investigation.
- Objective 4: Detect anomalies in order timestamps and promo usage patterns, and use them to flag high-risk merchants and users.

## 2. Data Sources
- [Dataset 1](link) – Food Orders (order status, timestamps, promo discount, cancellation reason, etc.)
- [Dataset 2](link) – Merchants (merchant ID, merchant name, cuisine type)
- [Dataset 3](link) – Users (user ID, account age, GrabUnlimited subscription status)

## 3. Technologies Used
- Programming Language: Python (Pandas, NumPy)
- Statistical Testing: SciPy (Shapiro-Wilk, Mann-Whitney U, T-Test)
- Visualization: Matplotlib, Seaborn, Plotly
- Version Control: Git
- Others: Jupyter Notebook

## 4. Project Structure

```
├── README.md          <- The top-level README for developers using this project.
|
├── data
│   ├── raw            <- Data from third party sources.
│   └── cleaned        <- The data that has been cleaned.
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── reports            <- Generated analysis as PowerPoint, PDF, LaTeX, etc.
|   ├── slide          <- Generated PowerPoint
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
└── src                <- Source code for use in this project.

```

## 5. Summary of Finding
### 5.1 Business Insight
**Data Quality Issues**
- The order_status column contained 7 different text variations that actually represented only 3 real statuses (Completed, Cancelled, Failed), caused by mixed language and inconsistent capitalization.
- 40.24% of all cancelled orders had no cancellation reason recorded, making it difficult for the operations team to distinguish genuine issues from fraud-related cancellations.
- The promo_discount column contained invalid placeholder values (e.g. -5000 and 9999999) that had to be cleaned before analysis.

**Fraud & Anomaly Findings**
- Cross-checking both anomaly signals (high cancellation rate and timestamp anomaly) narrowed the list down to 22 "super high-risk" merchants, resulting in Rp 1,260,000 in confirmed subsidy losses across 42 fraudulent orders. Each order came from a different user account, suggesting a Sybil attack in which multiple fake accounts were created to distribute fraudulent orders and avoid detection.
- About 3.20% of all orders, or 4.00% of completed orders, were recorded as completed before the driver arrived, an impossible sequence in a real delivery. The anomaly was heavily concentrated among orders using the highest promo discount of Rp 30,000, with a rate of 24.95%, far higher than other promo tiers. This indicates a likely system loophole being exploited for promo abuse, exposing up to Rp 288,090,000 in subsidy risk across 5,635 affected merchants.
- About 40.24% of cancelled orders had no recorded cancellation reason. This missing information makes it difficult to distinguish legitimate cancellations from potentially fraudulent ones, reducing the effectiveness of fraud detection and slowing investigation and reporting.
- A total of 36 merchants were identified as high risk due to an unusually high cancellation and failure rate among GrabUnlimited orders, resulting in an estimated Rp 6,610,000 in wasted promo subsidy. Combined with the concentration of timestamp anomalies in the highest promo tier of Rp 30,000, this indicates that fraud is more likely to target higher subsidy values because they offer greater financial returns per fraudulent transaction.

### 5.2 Actionable Recommendation
- **Prioritize investigation** of the 22 "super high-risk" merchants and their associated 42 user accounts, since they are confirmed on two independent fraud signals at once.
- **Add a system-level validation rule** to prevent an order from being marked "Completed" before the driver's arrival timestamp is recorded, closing the loophole being exploited for promo abuse.
- **Make the cancellation reason field mandatory** at the point of cancellation to eliminate the current 40.24% missing-reason rate and support faster fraud investigation.
- **Monitor high-value promo tiers (e.g. Rp 30,000) more closely**, since fraud activity is disproportionately concentrated in the largest subsidy amount.

## 6. Contact
- Name: Zian Carlos Wong, Valencia
- Email: avlentcia@gmail.com
- Linkedin: