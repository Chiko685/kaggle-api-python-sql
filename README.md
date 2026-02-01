Retail Orders Analytics Project
## 📊 Project Overview
This project performs end-to-end data engineering and analytics on a retail orders dataset. It includes data extraction, cleaning, transformation, loading into PostgreSQL, and advanced SQL analytics to uncover business insights about sales performance, regional trends, and product profitability.

## 🗂️ File Structure
<img width="444" height="118" alt="Screenshot 2026-02-02 at 00 19 36" src="https://github.com/user-attachments/assets/d5ef46d7-9685-4290-bb94-290481e043d8" />


## 🔍 Data Source
a. Dataset: Retail Orders Dataset from Kaggle

b. Original Format: ZIP archive containing orders.csv

c. Records: 9,994 order transactions

d. Time Period: January 2022 - December 2023

## 🧹 Data Cleaning & Transformation Process
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
order_id, order_date, ship_mode, segment, country, city, state, postal_code, region, category, sub_category, product_id, quantity, discount, sale_price, profit

5. Database Loading
a. Transferred cleaned data to PostgreSQL using SQLAlchemy

b. Connection parameters configured for local PostgreSQL instance (port 5433)

c. Created orders table with to_sql() method (if_exists='replace')

## 💡 Key Analytical Queries (project1.sql)
1. Schema Optimization
<img width="501" height="137" alt="Screenshot 2026-02-01 at 23 12 53" src="https://github.com/user-attachments/assets/393af200-df5d-4fcf-a142-4659d20c3aea" />

2. Top Revenue Generators
<img width="501" height="189" alt="Screenshot 2026-02-01 at 23 13 35" src="https://github.com/user-attachments/assets/a5336d56-4e86-443c-92af-8b28ad9b21d7" />

3. Regional Performance Analysis
<img width="566" height="300" alt="Screenshot 2026-02-01 at 23 15 11" src="https://github.com/user-attachments/assets/664e789d-bcec-4d01-811f-8f604d1c3629" />

4. Year-over-Year Growth Analysis
<img width="566" height="355" alt="Screenshot 2026-02-01 at 23 16 22" src="https://github.com/user-attachments/assets/930fe443-c2bf-4c31-9fde-63a4e817d4b9" />

5. Category Performance by Month
<img width="566" height="355" alt="Screenshot 2026-02-01 at 23 16 50" src="https://github.com/user-attachments/assets/2270a48f-d7d9-4914-9030-385d84d00a3a" />

6. Growth Analysis
<img width="611" height="418" alt="Screenshot 2026-02-01 at 23 17 55" src="https://github.com/user-attachments/assets/7c207575-88a9-4ce1-92fd-71b5045203c1" />


##⚙️ Technical Requirements
## Python Dependencies

--pip install kaggle pandas sqlalchemy psycopg2-binary pyodbc

## Database Requirements
1. PostgreSQL 18.0+ (tested on Postgres.app for macOS)
2. Running instance on localhost:5433
3. Database: postgres
4. Credentials configured in notebook (update before execution):

username = 'postgres'
password = 'your_password_here'

## 🚀 How to Run
1. Set up Kaggle API (if downloading dataset):
a. Create kaggle.json in ~/.kaggle/ with API credentials
b. Set permissions: chmod 600 ~/.kaggle/kaggle.json

2. Execute ETL Pipeline:
a. Open Project retail orders.ipynb in Jupyter

b. Run all cells sequentially (handles ZIP extraction → cleaning → DB loading)


3. Run Analytics:
a. Connect to PostgreSQL database using DBeaver/pgAdmin

b. Execute queries from project1.sql for business insights

## 📈 Key Business Insights Enabled
a. Identify top-performing products by revenue and region

b. Track YoY sales growth at monthly granularity

c. Discover seasonal patterns per product category

d. Pinpoint high-growth sub-categories for inventory planning

e. Analyze profitability at transaction level (profit = sale_price - cost_price)

f. Regional performance benchmarking for sales strategy optimization
