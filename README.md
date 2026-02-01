Retail Orders Analytics Project
## 📊 Project Overview
This project performs end-to-end data engineering and analytics on a retail orders dataset. It includes data extraction, cleaning, transformation, loading into PostgreSQL, and advanced SQL analytics to uncover business insights about sales performance, regional trends, and product profitability.
## 🗂️ File Structure

project/
├── Project retail orders.ipynb    # Data cleaning & ETL pipeline (Jupyter Notebook)
├── project1.sql                   # Analytical SQL queries
└── README.md                      # This documentation file
## 🔍 Data Source
Dataset: Retail Orders Dataset from Kaggle
Original Format: ZIP archive containing orders.csv
Records: 9,994 order transactions
Time Period: January 2022 - December 2023
🧹 Data Cleaning & Transformation Process
The Jupyter Notebook (Project retail orders.ipynb) implements a comprehensive ETL pipeline:
1. Data Acquisition

# Download dataset from Kaggle
!kaggle datasets download ankitbansal06/retail-orders -f orders.csv

2. File Format Handling
- Detected that downloaded "CSV" was actually a ZIP archive (identified by PK header signature)
- Extracted actual CSV file from archive using Python's zipfile module

3. Data Cleaning Steps
<img width="805" height="350" alt="Screenshot 2026-02-01 at 22 43 17" src="https://github.com/user-attachments/assets/808dcde6-57a5-420a-be42-4b77d84a0e2b" />


4. Final Schema (16 columns)
order_id, order_date, ship_mode, segment, country, city, state, 
postal_code, region, category, sub_category, product_id, quantity, 
discount, sale_price, profit

5. Database Loading
- Transferred cleaned data to PostgreSQL using SQLAlchemy
- Connection parameters configured for local PostgreSQL instance (port 5433)
- Created orders table with to_sql() method (if_exists='replace')

## 💡 Key Analytical Queries (project1.sql)
A. Schema Optimization

1. Convert order_date from timestamp to date type
2. ALTER TABLE orders 
3. ALTER COLUMN order_date TYPE DATE USING order_date::DATE;

B. Top Revenue Generators
sql
123456
-- Top 10 highest revenue generating products
SELECT product_id, SUM(sale_price::NUMERIC) AS sales
FROM orders
GROUP BY product_id
ORDER BY sales DESC
LIMIT 10;
3. Regional Performance Analysis
sql
1234567891011
-- Top 5 highest-selling products per region using window functions
WITH rank_region AS (
  SELECT region, product_id, SUM(sale_price::NUMERIC) AS sales
  FROM orders
  GROUP BY region, product_id
)
SELECT * FROM (
  SELECT *, DENSE_RANK() OVER (PARTITION BY region ORDER BY sales DESC) AS ranking
  FROM rank_region
) AS sub_rank

4. Year-over-Year Growth Analysis
sql
12345678910111213141516
-- Month-over-month sales comparison: 2022 vs 2023
WITH cte AS (
  SELECT 
    SUM(sale_price::NUMERIC) AS sales,
    EXTRACT(MONTH FROM order_date) AS month,
    EXTRACT(YEAR FROM order_date) AS year
  FROM orders
  GROUP BY month, year
)
SELECT 

5. Category Performance by Month
sql
123456789101112131415
-- Highest sales month for each product category
WITH category AS (
  SELECT 
    category,
    EXTRACT(MONTH FROM order_date) AS month,
    SUM(sale_price::NUMERIC) AS sales
  FROM orders
  GROUP BY category, month
)
SELECT * FROM (

6. Growth Analysis
sql
12345678910111213141516171819202122
-- Highest growth sub-category (2023 vs 2022)
WITH sub_category AS (
  SELECT 
    sub_category,
    EXTRACT(YEAR FROM order_date) AS year,
    SUM(sale_price::NUMERIC) AS sales
  FROM orders
  GROUP BY sub_category, year
),
cte_2 AS (

⚙️ Technical Requirements
Python Dependencies
bash
1
pip install kaggle pandas sqlalchemy psycopg2-binary pyodbc
Database Requirements
PostgreSQL 18.0+ (tested on Postgres.app for macOS)
Running instance on localhost:5433
Database: postgres
Credentials configured in notebook (update before execution):
python
12
username = 'postgres'
password = 'your_password_here'
🚀 How to Run
Set up Kaggle API (if downloading dataset):
Create kaggle.json in ~/.kaggle/ with API credentials
Set permissions: chmod 600 ~/.kaggle/kaggle.json
Execute ETL Pipeline:
Open Project retail orders.ipynb in Jupyter
Run all cells sequentially (handles ZIP extraction → cleaning → DB loading)
Run Analytics:
Connect to PostgreSQL database using DBeaver/pgAdmin
Execute queries from project1.sql for business insights
📈 Key Business Insights Enabled
Identify top-performing products by revenue and region
Track YoY sales growth at monthly granularity
Discover seasonal patterns per product category
Pinpoint high-growth sub-categories for inventory planning
Analyze profitability at transaction level (profit = sale_price - cost_price)
Regional performance benchmarking for sales strategy optimization
