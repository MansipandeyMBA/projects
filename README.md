Retail Sales Analysis – SQL Project
📌 Project Overview
Title: Retail Sales Analysis
Difficulty: Beginner
Database: p1_retail_db

This project demonstrates essential SQL skills for aspiring data analysts. It involves setting up a retail sales database, cleaning and exploring data, and answering business questions through SQL queries. It's a perfect starting point for anyone looking to build a strong foundation in SQL-based data analysis.

🎯 Project Objectives
Database Setup: Create and populate a retail sales database.

Data Cleaning: Identify and remove records with missing or null values.

Exploratory Data Analysis (EDA): Gain insights into customer behavior and sales patterns.

Business Analysis: Solve real-world business queries using SQL.

🏗️ Project Structure
1. Database Setup
Create Database: p1_retail_db

Create Table: retail_sales with fields like transaction ID, sale date, customer ID, gender, age, product category, quantity, price per unit, COGS, and total sale amount.

sql
Copy
Edit
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
2. Data Exploration and Cleaning
Record Count: Total number of transactions.

Unique Customers: Number of distinct customers.

Product Categories: List of unique product categories.

Null Check and Cleaning: Detect and remove incomplete records.

sql
Copy
Edit
-- Count records
SELECT COUNT(*) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Unique categories
SELECT DISTINCT category FROM retail_sales;

-- Check for missing data
SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR 
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Delete missing data
DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR 
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
3. Data Analysis – Key SQL Queries
Sales on a specific date:

sql
Copy
Edit
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
High quantity clothing sales in Nov 2022:

sql
Copy
Edit
SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
Total sales per category:

sql
Copy
Edit
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
Average age of Beauty category customers:

sql
Copy
Edit
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
Transactions with sales over 1000:

sql
Copy
Edit
SELECT * FROM retail_sales
WHERE total_sale > 1000;
Transactions count by gender and category:

sql
Copy
Edit
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
Best selling month each year (average sale):

sql
Copy
Edit
SELECT year, month, avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY year, month
) AS ranked_sales
WHERE rank = 1;
Top 5 customers by sales:

sql
Copy
Edit
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
Unique customers per category:

sql
Copy
Edit
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
Orders by shift (Morning, Afternoon, Evening):

sql
Copy
Edit
WITH hourly_sale AS (
    SELECT *,
           CASE
               WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
               WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
               ELSE 'Evening'
           END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
📈 Key Insights
Customer Demographics: Diverse customer base across age groups and genders.

High-Value Transactions: Significant number of premium sales (>1000).

Seasonality: Identified peak sales months and periods.

Customer Loyalty: Top customers and best-performing product categories identified.

📋 Reports
Sales Summary: Overall sales performance, broken down by customer segments and categories.

Trend Analysis: Monthly and shift-based sales patterns.

Customer Insights: Top customers and unique customer trends.

✅ Conclusion
This project provides a comprehensive walkthrough of SQL fundamentals for retail data analysis, from database setup to answering critical business questions. The insights derived can help inform real-world business strategies related to sales optimization, customer targeting, and inventory management.

