

# 🛒 Customer Behaviour Analysis — End-to-End Data Analytics Project

<p align="left">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white"/>
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white"/>
</p>

---

## 📌 Overview

This end-to-end data analytics project analyses **customer purchasing behaviour** to uncover patterns in spending, product preferences, demographic segments, and subscription trends. The goal is to help business stakeholders understand which customer segments drive the most revenue, how subscription status affects purchasing, and where opportunities exist to improve engagement and retention.

The project covers the full analytics workflow — from raw data ingestion and cleaning in Python, to SQL-based querying in PostgreSQL, to an interactive Power BI dashboard and a presentation-ready report.

---

## 📁 Dataset

| Field | Detail |
|-------|--------|
| **Source** | Customer transactions dataset |
| **Format** | CSV |
| **Key columns** | `customer_id`, `age_group`, `gender`, `category`, `purchase_amount`, `subscription_status`, `shipping_type`, `review_rating` |
| **Records** | `[INSERT: e.g. 10,000 rows]` |
| **Time period** | `[INSERT: e.g. Jan 2023 – Dec 2023]` |

---

## 🛠️ Tools & Technologies

| Layer | Tool |
|-------|------|
| Data loading & EDA | Python (Pandas, NumPy, Matplotlib, Seaborn), Jupyter Notebook |
| Data cleaning | Python (Pandas) |
| Data storage & querying | PostgreSQL |
| Dashboard & visualisation | Power BI (DAX, Power Query) |
| Report | PDF / Word report |
| Presentation | Gamma (AI-powered PPT) |
| Version control | Git & GitHub |

---

## 🔢 Project Steps

### Step 1 — Load Dataset in Python

```python
import pandas as pd

df = pd.read_csv('customer_behaviour.csv')
print(df.shape)
print(df.head())
```

- Loaded the raw CSV into a Pandas DataFrame
- Inspected shape, column names, and data types
- Checked for nulls, duplicates, and inconsistent values

---

### Step 2 — Exploratory Data Analysis (EDA)

```python
# Distribution of purchase amounts
import matplotlib.pyplot as plt
import seaborn as sns

sns.histplot(df['purchase_amount'], bins=30)
plt.title('Distribution of Purchase Amount')
plt.show()

# Subscription status breakdown
df['subscription_status'].value_counts().plot(kind='pie', autopct='%1.1f%%')
plt.show()
```

Key EDA findings:
- Distribution of customers by age group, gender, and subscription status
- Purchase amount spread across product categories
- Correlation between review rating and purchase amount
- Category-level sales volume patterns

---

### Step 3 — Data Cleaning

```python
# Drop duplicates
df.drop_duplicates(inplace=True)

# Handle missing values
df['review_rating'].fillna(df['review_rating'].median(), inplace=True)

# Standardise text columns
df['category'] = df['category'].str.strip().str.title()
df['gender'] = df['gender'].str.strip().str.title()
df['subscription_status'] = df['subscription_status'].str.strip().str.lower()

# Fix data types
df['purchase_amount'] = df['purchase_amount'].astype(float)
```

Cleaning steps performed:
- Removed duplicate records
- Imputed missing values in `review_rating` with median
- Standardised text casing in `category`, `gender`, and `subscription_status`
- Validated and corrected data types

---

### Step 4 — Load into PostgreSQL

```python
from sqlalchemy import create_engine

engine = create_engine('postgresql://username:password@localhost:5432/customer_db')
df.to_sql('customer_data', engine, if_exists='replace', index=False)
print("Data loaded successfully.")
```

---

### Step 5 — SQL Analysis (PostgreSQL)

**Total customers and average purchase amount:**
```sql
SELECT
    COUNT(DISTINCT customer_id)     AS total_customers,
    ROUND(AVG(purchase_amount), 2)  AS avg_purchase_amount,
    ROUND(AVG(review_rating), 2)    AS avg_review_rating
FROM customer_data;
```

**Revenue by product category:**
```sql
SELECT
    category,
    COUNT(customer_id)              AS total_orders,
    ROUND(SUM(purchase_amount), 2)  AS total_revenue
FROM customer_data
GROUP BY category
ORDER BY total_revenue DESC;
```

**Revenue and sales by age group:**
```sql
SELECT
    age_group,
    COUNT(customer_id)              AS total_customers,
    ROUND(SUM(purchase_amount), 2)  AS total_revenue
FROM customer_data
GROUP BY age_group
ORDER BY total_revenue DESC;
```

**Subscription status breakdown:**
```sql
SELECT
    subscription_status,
    COUNT(customer_id)                                          AS customer_count,
    ROUND(COUNT(customer_id) * 100.0 / SUM(COUNT(*)) OVER(),2) AS percentage
FROM customer_data
GROUP BY subscription_status;
```

**Revenue by gender:**
```sql
SELECT
    gender,
    ROUND(SUM(purchase_amount), 2)  AS total_revenue,
    ROUND(AVG(purchase_amount), 2)  AS avg_purchase
FROM customer_data
GROUP BY gender
ORDER BY total_revenue DESC;
```

**Revenue by shipping type:**
```sql
SELECT
    shipping_type,
    COUNT(customer_id)              AS orders,
    ROUND(SUM(purchase_amount), 2)  AS total_revenue
FROM customer_data
GROUP BY shipping_type
ORDER BY total_revenue DESC;
```

---

### Step 6 — Power BI Dashboard

Connected Power BI to the cleaned dataset and built an interactive single-page dashboard with the following visuals:

| Visual | Type | Fields |
|--------|------|--------|
| KPI cards | Card visual | Total customers, Avg purchase amount, Avg review rating |
| Subscription split | Donut chart | `subscription_status`, Count of `customer_id` |
| Revenue by category | Clustered column chart | `category`, Sum of `purchase_amount` |
| Sales volume by category | Clustered column chart | `category`, Count of `customer_id` |
| Revenue by age group | Clustered bar chart | `age_group`, Sum of `purchase_amount` |
| Sales by age group | Clustered bar chart | `age_group`, Count of `customer_id` |

**Slicers (interactive filters):**
- Subscription Status
- Gender
- Product Category
- Shipping Type

> 📎 Dashboard file: `customer_behaviour.pbix`

---

### Step 7 — Report & Presentation

- **Report:** Written summary of methodology, findings, and business recommendations (PDF/Word)
- **Presentation:** Built using [Gamma](https://gamma.app) — AI-powered slides covering project overview, key insights, dashboard walkthrough, and next steps

---

## 📊 Key Results

> ✏️ *Fill in the values below by opening your Power BI file and reading the KPI cards and charts*

| Metric | Value |
|--------|-------|
| Total customers | `[INSERT]` |
| Average purchase amount | `$[INSERT]` |
| Average review rating | `[INSERT] / 5` |
| Highest revenue category | `[INSERT e.g. Electronics]` |
| Highest revenue age group | `[INSERT e.g. 35–44]` |
| Subscription rate (active) | `[INSERT]%` |
| Top shipping type by revenue | `[INSERT e.g. Express]` |

**Key findings:**
- 📦 **[INSERT top category]** generated the highest total revenue across all product categories
- 👥 **[INSERT age group]** customers were the most valuable demographic by total spend
- 🔄 **[INSERT X]%** of customers held an active subscription, indicating `[strong/moderate/low]` retention
- 🚚 Customers using **[INSERT shipping type]** shipping had the highest average purchase amounts
- ⭐ Overall average review rating of **[INSERT]** suggests `[high/moderate]` customer satisfaction

---

## 💡 Business Recommendations

1. **Double down on the top revenue category** — allocate more marketing budget and inventory to `[INSERT category]`
2. **Re-engage inactive subscribers** — target unsubscribed customers with personalised offers to improve retention
3. **Focus campaigns on the highest-value age group** — tailor messaging and promotions toward `[INSERT age group]` demographics
4. **Investigate low-revenue categories** — explore whether pricing, placement, or product quality is driving lower performance
5. **Review shipping strategy** — analyse whether preferred shipping types correlate with higher satisfaction scores

---

## 📂 Project Structure

```
customer-behaviour-analysis/
│
├── data/
│   └── customer_behaviour.csv          # Raw dataset
│
├── notebooks/
│   └── 01_eda_and_cleaning.ipynb       # Python EDA & data cleaning
│
├── sql/
│   └── analysis_queries.sql            # All PostgreSQL queries
│
├── dashboard/
│   └── customer_behaviour.pbix         # Power BI dashboard file
│
├── report/
│   └── customer_behaviour_report.pdf   # Written analysis report
│
├── presentation/
│   └── customer_behaviour_slides.pdf   # Gamma presentation export
│
└── README.md
```

---

## ▶️ How to Run

### 1. Clone the repository
```bash
git clone https://github.com/YourUsername/customer-behaviour-analysis.git
cd customer-behaviour-analysis
```

### 2. Install Python dependencies
```bash
pip install pandas numpy matplotlib seaborn sqlalchemy psycopg2 jupyter
```

### 3. Run the Jupyter Notebook
```bash
jupyter notebook notebooks/01_eda_and_cleaning.ipynb
```

### 4. Set up PostgreSQL
```bash
# Create the database
createdb customer_db

# Run the analysis queries
psql -d customer_db -f sql/analysis_queries.sql
```

### 5. Open the Power BI Dashboard
- Open `dashboard/customer_behaviour.pbix` in **Power BI Desktop**
- Refresh the data source connection if needed
- Use the slicers (Subscription Status, Gender, Category, Shipping Type) to explore segments

---

## 👤 Author

**Mukul Madhav Cheekati**
📍 Cleveland, OH | M.S. Business Analytics, Trine University *(Expected May 2026)*

<p align="left">
  <a href="https://linkedin.com/in/mukulmadhav-cheekati">
    <img src="https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/>
  </a>
  <a href="mailto:Mukulmadhav113@gmail.com">
    <img src="https://img.shields.io/badge/Email-Contact-D14836?style=for-the-badge&logo=gmail&logoColor=white"/>
  </a>
</p>

---

> *"The goal is to turn data into information, and information into insight."* — Carly Fiorina
