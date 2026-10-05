# 🍽️ Zomato End-to-End Business Analysis using Python

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas)
![NumPy](https://img.shields.io/badge/NumPy-Numerical%20Analysis-013243?logo=numpy)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Visualization-4c72b0)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)

## 📌 Project Overview

This project performs an **end-to-end exploratory and business analysis of Zomato data** using Python.

The dataset contains three related business tables:

1. **Customer** — customer profile and acquisition information
2. **Restaurants** — restaurant, cuisine, city and rating information
3. **Orders** — transaction-level order information

The objective is not only to explore the data, but to answer practical **business questions** related to revenue, customers, restaurants, cities, cuisines, discounts, payments, order status and growth opportunities.

---

# 🎯 Business Objective

The main objective is to convert raw Zomato transactional data into actionable business insights.

The analysis focuses on:

- Revenue performance
- Order volume and trends
- Customer behavior
- Repeat customers
- Customer acquisition channels
- Restaurant performance
- Cuisine performance
- City-level opportunities
- Discounts and pricing
- Payment behavior
- Order status
- Restaurant ratings
- Business growth opportunities

---

# 📂 Dataset Structure

The Excel workbook contains three sheets.

## 1. Customer Table

| Column | Description |
|---|---|
| `customer_id` | Unique customer identifier |
| `customer_name` | Customer name |
| `city` | Customer city |
| `signup_time` | Customer registration date |
| `acquisition_channel` | Channel through which customer was acquired |

### Example Acquisition Channels

- Organic
- Instagram Ads
- WhatsApp Campaign
- Other marketing channels

---

## 2. Restaurants Table

| Column | Description |
|---|---|
| `restaurant_id` | Unique restaurant identifier |
| `restaurant_name` | Restaurant name |
| `cuisine` | Primary cuisine |
| `city` | Restaurant city |
| `avg_rating` | Average restaurant rating |

---

## 3. Orders Table

| Column | Description |
|---|---|
| `order_id` | Unique order identifier |
| `customer_id` | Customer who placed the order |
| `restaurant_id` | Restaurant receiving the order |
| `order_timestamp` | Date of order |
| `order_amount` | Order value |
| `discount_amount` | Discount provided |
| `delivery_fee` | Delivery fee |
| `payment_mode` | Payment method |
| `order_status` | Current order status |

---

# 🔗 Data Model

The three tables are connected using primary/foreign-key relationships.

```text
CUSTOMER
   |
   | customer_id
   |
   v
ORDERS
   |
   | restaurant_id
   |
   v
RESTAURANTS
```

### Relationships

```text
customer.customer_id
        |
        | 1 : Many
        v
orders.customer_id


restaurants.restaurant_id
        |
        | 1 : Many
        v
orders.restaurant_id
```

The project validates these relationships before performing the business analysis.

---

# 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| Pandas | Data cleaning, transformation and analysis |
| NumPy | Numerical calculations |
| Matplotlib | Business charts |
| Seaborn | Statistical visualization |
| Jupyter Notebook | Interactive analysis |
| Excel | Source dataset |
| GitHub | Project documentation and portfolio |

---

# 📁 Project Structure

Recommended GitHub repository structure:

```text
Zomato-Python-Business-Analysis/
│
├── data/
│   └── Zomato_dataset.xlsx
│
├── notebooks/
│   └── Zomato_End_to_End_Business_Analysis.ipynb
│
├── reports/
│   └── README.md
│
├── images/
│   ├── monthly_orders.png
│   ├── monthly_revenue.png
│   ├── city_revenue.png
│   ├── cuisine_revenue.png
│   ├── acquisition_channel.png
│   ├── payment_analysis.png
│   └── correlation_matrix.png
│
└── README.md
```

> **Tip:** For a public GitHub repository, avoid uploading sensitive or proprietary customer information.

---

# 🔄 Project Workflow

The project follows this analytical workflow:

```text
Raw Excel Data
      ↓
Data Loading
      ↓
Data Quality Check
      ↓
Data Cleaning
      ↓
Relationship Validation
      ↓
Table Joining
      ↓
Feature Engineering
      ↓
Exploratory Data Analysis
      ↓
Business Questions
      ↓
Visualization
      ↓
Insights
      ↓
Recommendations
```

---

# 🧹 1. Data Loading

The Excel workbook is loaded using Pandas.

```python
import pandas as pd

FILE_PATH = "Zomato_dataset.xlsx"

customer = pd.read_excel(FILE_PATH, sheet_name="customer")
restaurants = pd.read_excel(FILE_PATH, sheet_name="restaurants")
orders = pd.read_excel(FILE_PATH, sheet_name="orders")
```

---

# 🧹 2. Data Cleaning

The following cleaning operations are performed:

### Column standardization

```python
df.columns = (
    df.columns
    .astype(str)
    .str.strip()
    .str.lower()
    .str.replace(r"\s+", "_", regex=True)
)
```

### Date conversion

```python
customer["signup_time"] = pd.to_datetime(
    customer["signup_time"],
    errors="coerce"
)

orders["order_timestamp"] = pd.to_datetime(
    orders["order_timestamp"],
    errors="coerce"
)
```

### Numeric conversion

```python
for col in [
    "order_amount",
    "discount_amount",
    "delivery_fee"
]:
    orders[col] = pd.to_numeric(
        orders[col],
        errors="coerce"
    )
```

### Duplicate removal

```python
df = df.drop_duplicates()
```

---

# 🔍 3. Data Quality Analysis

The project checks:

- Number of rows
- Number of columns
- Missing values
- Duplicate records
- Unique values
- Data types
- Primary key uniqueness
- Foreign key validity

Example:

```python
df.isnull().sum()
```

and:

```python
df.duplicated().sum()
```

---

# 🔗 4. Master Analytical Dataset

The three tables are joined to create a single analytical dataset.

```python
master = (
    orders
    .merge(
        customer,
        on="customer_id",
        how="left"
    )
    .merge(
        restaurants,
        on="restaurant_id",
        how="left"
    )
)
```

This allows analysis across:

- Customer
- Restaurant
- City
- Cuisine
- Order
- Payment
- Discount
- Rating
- Acquisition channel

---

# 🧮 5. Feature Engineering

Several business metrics are created.

## Gross Order Value

```python
master["gross_order_value"] = (
    master["order_amount"]
    + master["delivery_fee"]
)
```

## Net Order Value

```python
master["net_order_value_after_discount"] = (
    master["order_amount"]
    - master["discount_amount"]
)
```

## Discount Percentage

```python
master["discount_pct"] = (
    master["discount_amount"]
    / master["order_amount"]
    * 100
)
```

## Order Month

```python
master["order_month"] = (
    master["order_timestamp"]
    .dt.to_period("M")
    .astype(str)
)
```

## Order Day

```python
master["order_day"] = (
    master["order_timestamp"]
    .dt.day_name()
)
```

---

# 📊 Business Questions

The notebook answers **20 business questions**.

---

## Q1. What is the overall business performance?

KPIs calculated:

- Total Orders
- Delivered Orders
- Total Revenue
- Total Discounts
- Total Delivery Fees
- Average Order Value
- Delivery Success Rate

### Business Value

Provides an executive-level overview of the platform's transaction performance.

---

## Q2. How are orders distributed by status?

Order statuses are analyzed to understand:

- Successful orders
- Cancelled orders
- Refunded orders
- Other operational outcomes

### Business Value

Helps identify potential operational problems and revenue leakage.

---

## Q3. How has order volume and revenue changed over time?

Monthly trends are analyzed for:

- Order growth
- Revenue growth
- Discounts
- Delivery fees

### Business Value

Helps identify:

- Growth periods
- Slow periods
- Seasonality
- Revenue trends

---

## Q4. Which cities generate the most orders and revenue?

City-level analysis compares:

- Orders
- Revenue
- Average Order Value
- Customers

### Business Value

Helps prioritize geographic expansion and marketing efforts.

---

## Q5. Which restaurants are the top revenue generators?

Restaurants are ranked based on:

- Revenue
- Orders
- Average Order Value
- Customers
- Rating

### Business Value

Identifies high-value restaurant partners.

---

## Q6. Which restaurants have high ratings and strong demand?

A combined performance score is created using:

- Order volume
- Customer count
- Restaurant rating

### Business Value

Identifies restaurants that are both popular and high quality.

---

## Q7. Which cuisines drive the most revenue?

Cuisine-level analysis measures:

- Orders
- Revenue
- Average Order Value
- Average Rating

### Business Value

Helps Zomato understand cuisine demand and build targeted campaigns.

---

## Q8. Which acquisition channels generate the most customers and revenue?

Channels are compared using:

- Customers
- Orders
- Revenue

### Business Value

Helps evaluate marketing effectiveness.

---

## Q9. Which acquisition channels produce high-value customers?

Revenue per customer and orders per customer are calculated.

```python
Revenue per Customer =
Total Revenue / Unique Customers
```

### Business Value

A channel bringing fewer customers may still be more profitable if those customers have higher lifetime value.

---

## Q10. What is the customer repeat-order behavior?

Customers are classified into:

```text
One-time Customer
        vs
Repeat Customer
```

### Repeat Customer Rate

```python
Repeat Customer Rate =
Repeat Customers / Total Customers × 100
```

### Business Value

Retention is generally more valuable than focusing only on acquisition volume.

---

## Q11. Who are the highest-value customers?

Customers are ranked by:

- Total revenue
- Number of orders
- Average order value

### Business Value

Supports:

- VIP programs
- Personalized offers
- Retention campaigns
- Customer segmentation

---

## Q12. How much revenue comes from repeat customers?

Revenue contribution is compared between:

- One-time customers
- Repeat customers

### Business Value

Shows how important customer retention is to total revenue.

---

## Q13. Are discounts increasing order value?

Orders are grouped into discount bands such as:

```text
0–5%
5–10%
10–20%
20–100%
100%+
```

Metrics compared:

- Orders
- Revenue
- Average Order Value
- Average Discount

### Business Value

Helps determine whether discounts are creating incremental demand or simply reducing revenue.

---

## Q14. What is the relationship between discounts and order amount?

Correlation and scatter analysis are used.

### Business Value

Helps understand whether larger orders naturally receive larger discounts.

---

## Q15. Which payment modes are most used?

Payment methods are compared using:

- Order volume
- Revenue
- Average Order Value
- Order share

### Business Value

Helps prioritize payment experiences and identify customer preferences.

---

## Q16. Which cities have the best restaurant quality?

Restaurant ratings are aggregated by city.

### Business Value

Helps identify markets with strong restaurant quality.

---

## Q17. Which cities have high demand but lower restaurant ratings?

An opportunity score combines:

- Demand scale
- Rating improvement potential

### Business Value

These locations may provide strong opportunities for merchant quality improvement.

---

## Q18. What days have the strongest order demand?

Orders are analyzed by:

- Monday
- Tuesday
- Wednesday
- Thursday
- Friday
- Saturday
- Sunday

### Business Value

Can support:

- Weekend promotions
- Delivery capacity planning
- Restaurant staffing
- Marketing scheduling

---

## Q19. What is the order value distribution?

Distribution analysis identifies:

- Typical order values
- High-value orders
- Low-value orders
- Potential outliers

### Business Value

Supports pricing and customer segmentation.

---

## Q20. What are the relationships among order economics?

Correlation analysis compares:

- Order Amount
- Discount Amount
- Delivery Fee
- Gross Order Value
- Net Order Value

### Business Value

Helps understand the relationship between pricing, discounts and transaction value.

---

# 📈 Visualization Strategy

The project uses multiple chart types.

## Bar Charts

Used for:

- Top cities
- Top restaurants
- Top cuisines
- Acquisition channels
- Payment modes

## Line Charts

Used for:

- Monthly orders
- Monthly revenue
- Business growth trends

## Histograms

Used for:

- Order amount distribution
- Rating distribution

## Boxplots

Used for:

- Outlier detection
- Rating comparison
- City comparison

## Scatter Plots

Used for:

- Discount vs order value
- Rating vs engagement

## Heatmaps

Used for:

- Correlation analysis

---

# 📌 Important KPIs

| KPI | Formula |
|---|---|
| Total Orders | Count of Order IDs |
| Total Revenue | Sum of Order Amount |
| Average Order Value | Revenue / Orders |
| Discount % | Discount / Order Amount × 100 |
| Repeat Customer Rate | Repeat Customers / Total Customers × 100 |
| Revenue per Customer | Revenue / Unique Customers |
| Orders per Customer | Orders / Unique Customers |
| Delivery Success Rate | Delivered Orders / Total Orders × 100 |
| Revenue Share | Segment Revenue / Total Revenue × 100 |

---

# 💡 Business Insights

The analysis framework is designed to identify the following types of insights:

### Revenue Concentration

A limited number of cities, restaurants or cuisines may contribute a disproportionate share of total revenue.

### Customer Retention

Repeat customers can contribute significantly more revenue than one-time customers.

### Acquisition Efficiency

The channel with the highest customer acquisition volume is not necessarily the channel with the highest revenue per customer.

### Restaurant Quality

High-demand restaurants with lower ratings should receive priority for quality improvement.

### Discount Efficiency

Large discounts should be evaluated against incremental orders and customer retention instead of being judged only by order volume.

### Geographic Opportunities

Cities with strong demand but weaker restaurant ratings can become targets for merchant quality initiatives.

### Cuisine Opportunities

High-revenue cuisines can be used for personalized discovery, promotional collections and restaurant acquisition strategies.

---

# 🚀 Business Recommendations

## 1. Improve Customer Retention

Create targeted campaigns for one-time customers.

Examples:

- Personalized coupons
- Cuisine-based recommendations
- Re-order reminders
- Loyalty rewards

---

## 2. Optimize Marketing Channels

Measure channels using:

```text
Customers
Orders
Revenue
Revenue per Customer
Repeat Rate
```

Do not optimize campaigns only on customer acquisition volume.

---

## 3. Reduce Unnecessary Discounts

Use data-driven discounting.

Instead of:

```text
Everyone gets 20% OFF
```

consider:

```text
High-value customer → personalized incentive
Inactive customer → reactivation offer
First-time customer → acquisition offer
Repeat customer → loyalty reward
```

---

## 4. Improve Low-Rated High-Demand Restaurants

Restaurants receiving many orders but maintaining low ratings should receive operational attention.

Possible actions:

- Food-quality monitoring
- Delivery-time analysis
- Customer feedback analysis
- Merchant coaching

---

## 5. Develop City-Level Strategies

Create a city scorecard containing:

```text
Orders
Revenue
AOV
Customers
Restaurant Count
Average Rating
Repeat Rate
```

Use this to prioritize growth investment.

---

## 6. Promote High-Performing Cuisines

Create curated collections such as:

```text
Top Rated Pizza
Best Budget Restaurants
Best North Indian
Trending Fast Food
Highest Rated Restaurants
```

---

## 7. Focus on High-Value Customers

Build VIP / loyalty programs around customers with:

- High order frequency
- High total spend
- Strong repeat behavior

---

## 8. Monitor Operational Leakage

Track:

- Cancelled orders
- Refunded orders
- Failed orders
- Low-rated restaurants

These can directly affect revenue and customer trust.

---

# 📊 Suggested Executive Dashboard

If this project is later converted into Power BI, recommended dashboard pages are:

## Page 1 — Executive Overview

KPIs:

- Total Revenue
- Total Orders
- AOV
- Customers
- Repeat Rate
- Delivery Success Rate

Visuals:

- Revenue Trend
- Order Trend
- Order Status
- Top Cities

---

## Page 2 — Customer Analytics

Visuals:

- One-time vs Repeat Customers
- Revenue by Customer Type
- Acquisition Channel
- Revenue per Customer
- Top Customers

---

## Page 3 — Restaurant Analytics

Visuals:

- Top Restaurants
- Restaurant Revenue
- Restaurant Rating
- Cuisine Performance
- City Performance

---

## Page 4 — Marketing & Discount Analytics

Visuals:

- Acquisition Channel
- Discount Band
- Revenue vs Discount
- AOV by Discount Band

---

## Page 5 — Geographic Analysis

Visuals:

- City Revenue
- City Orders
- City Rating
- City Opportunity Score

---

# 🧪 Statistical / Analytical Techniques

The project demonstrates:

- Descriptive statistics
- Aggregation
- GroupBy analysis
- Joins / Merge
- Correlation analysis
- Ranking
- Quantile segmentation
- Outlier detection
- Time-series aggregation
- Customer segmentation
- Opportunity scoring

---

# 🧠 Example NumPy Usage

NumPy is used for numerical calculations and scoring.

Example:

```python
master["discount_pct"] = np.where(
    master["order_amount"] > 0,
    master["discount_amount"]
    / master["order_amount"] * 100,
    0
)
```

Opportunity scoring:

```python
city_opportunity["Opportunity_Score"] = (
    0.6 * city_opportunity["Scale_Score"]
    + 0.4 * city_opportunity["Improvement_Score"]
)
```

---

# 🐼 Example Pandas Operations

### GroupBy

```python
master.groupby("city")["order_amount"].sum()
```

### Aggregation

```python
master.groupby("cuisine").agg(
    Orders=("order_id", "count"),
    Revenue=("order_amount", "sum"),
    Avg_Order_Value=("order_amount", "mean")
)
```

### Merge

```python
orders.merge(
    customer,
    on="customer_id",
    how="left"
)
```

### Ranking

```python
restaurant_perf.sort_values(
    "Revenue",
    ascending=False
)
```

---

# 📉 Example Seaborn Visualization

```python
sns.barplot(
    data=city_perf.head(10).reset_index(),
    x="Revenue",
    y="customer_city"
)

plt.title("Top 10 Cities by Revenue")
plt.show()
```

---

# ▶️ How to Run the Project

## Step 1 — Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/Zomato-Python-Business-Analysis.git
```

## Step 2 — Open Project

```bash
cd Zomato-Python-Business-Analysis
```

## Step 3 — Install Dependencies

```bash
pip install pandas numpy matplotlib seaborn openpyxl jupyter
```

## Step 4 — Start Jupyter Notebook

```bash
jupyter notebook
```

## Step 5 — Open

```text
Zomato_End_to_End_Business_Analysis.ipynb
```

## Step 6 — Run All Cells

From Jupyter:

```text
Kernel → Restart & Run All
```

---

# 📦 Requirements

Create a `requirements.txt` file:

```text
pandas
numpy
matplotlib
seaborn
openpyxl
jupyter
```

Install:

```bash
pip install -r requirements.txt
```

---

# 🔐 Data Privacy

The dataset contains customer-related fields.

For a public GitHub portfolio:

- Do not publish personally identifiable customer information.
- Use anonymized/sample data.
- Do not expose private business information.
- Consider replacing names with synthetic identifiers before publishing.

---

# 🎓 Skills Demonstrated

This project demonstrates practical skills for:

### Data Analyst

- Python
- Pandas
- NumPy
- Data Cleaning
- EDA
- Data Visualization
- Business Analysis
- KPI Development

### MIS Analyst

- Reporting
- KPI tracking
- Trend analysis
- Business performance monitoring
- Operational analysis

### Business Analyst

- Business problem solving
- Requirement-oriented analysis
- Insight generation
- Recommendation development
- Decision support

---

# 💼 Resume Project Description

### Zomato Business Analysis — Python

> Performed end-to-end analysis of 50,000+ Zomato orders by integrating customer, restaurant and transaction datasets using Python. Conducted data cleaning, exploratory data analysis, customer retention analysis, revenue analysis, restaurant and cuisine performance analysis, acquisition-channel evaluation and discount analysis using Pandas, NumPy, Matplotlib and Seaborn. Generated actionable recommendations for customer retention, targeted promotions, restaurant quality improvement and geographic growth.

---

# 💬 Interview Explanation

If an interviewer asks:

### "Tell me about your Zomato project."

You can explain:

> "I worked on a Zomato business analytics project using Python. The dataset contained three related tables — customers, restaurants and orders. I first performed data cleaning and validated the relationships between the tables. Then I created a master analytical dataset using Pandas merge operations.  
>
> After that, I analyzed revenue, order trends, customer repeat behavior, acquisition channels, restaurant performance, cuisines, discounts, payment methods and city-level performance. I used NumPy for numerical calculations and Matplotlib and Seaborn for visualization.  
>
> Finally, I converted the analytical findings into business recommendations around customer retention, marketing-channel optimization, targeted discounts, restaurant quality and city-level growth."

---

# ⭐ Key Takeaway

This project demonstrates an important Data Analyst workflow:

```text
DATA
 ↓
CLEAN
 ↓
UNDERSTAND
 ↓
ANALYZE
 ↓
VISUALIZE
 ↓
FIND INSIGHTS
 ↓
RECOMMEND ACTION
```

The goal is not simply to create charts.

The goal is to answer:

> **"What is happening in the business, why is it happening, and what should the business do next?"**

---

# 👨‍💻 Author

**Dheeraj Sharma**

Aspiring Data Analyst | Python | SQL | Power BI | Excel

### Core Analytics Skills

```text
Python
Pandas
NumPy
Matplotlib
Seaborn
SQL
Power BI
Advanced Excel
Data Visualization
Business Analytics
```

---

# 📜 License

This project is intended for **educational, portfolio and demonstration purposes**.

If the underlying dataset is subject to separate licensing or usage restrictions, follow the original dataset provider's terms.

---

## ⭐ If you find this project useful

Consider giving the repository a ⭐ on GitHub.
