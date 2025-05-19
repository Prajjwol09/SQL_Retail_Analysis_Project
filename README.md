# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
 **Database**: `sql_db_2`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. 

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sql_db_2`.
- **Table Creation**: A table named `Sales_tbl` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sql_db_2;

-- creating table
CREATE TABLE Sales_tbl (
	transactions_id  INT PRIMARY KEY,
	sale_date DATE,
	sale_time  TIME,
	customer_id INT,
	gender VARCHAR(20),
	age INT,
	category VARCHAR(20),
	quantity INT,
	price_per_unit FLOAT,
	cogs FLOAT,
	total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
**DATA CLEANING**
SELECT COUNT(*) FROM SALES_TBL;

SELECT * FROM SALES_TBL;

SELECT * FROM SALES_TBL
WHERE TRANSACTIONS_ID IS NULL;

**CHECKING NULL VALUES**
SELECT * FROM SALES_TBL
WHERE 
	transactions_id IS NULL
OR
    sale_date IS NULL
OR
    sale_time IS NULL
OR
    customer_id IS NULL
OR
    gender IS NULL
OR
    age IS NULL
OR
    category IS NULL
OR
    quantity IS NULL
OR
    price_per_unit IS NULL
OR
    cogs IS NULL
OR
    total_sale IS NULL;

**DELETING NULL VALUES**
DELETE FROM SALES_TBL
WHERE
transactions_id IS NULL
OR
    sale_date IS NULL
OR
    sale_time IS NULL
OR
    customer_id IS NULL
OR
    gender IS NULL
OR
    age IS NULL
OR
    category IS NULL
OR
    quantity IS NULL
OR
    price_per_unit IS NULL
OR
    cogs IS NULL
OR
    total_sale IS NULL;

**DATA EXPLORATION**
**CHECKING SALES**
SELECT COUNT(*) AS total_sales from sales_tbl;

**CHECKING NUMBER OF UNIQUE CUSTOMERS**
SELECT COUNT(DISTINCT customer_id) AS total_customers from sales_tbl;

**CHECKING CATEGORIES**
SELECT DISTINCT(category) from sales_tbl;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:
```sql
-- RETRIEVING ALL THE SALES MADE ON '2022-11-05'
SELECT transactions_id, category FROM SALES_TBL
WHERE sale_date = '2022-11-05';


-- RETRIEVING TRANSACTIONS WHERE CATEGORY IS 'CLOTHING' AND QUANTITY SOLD IS MORE THAN OR EQUALS TO 4 IN THE MONTH OF NOV-2022
SELECT * FROM SALES_TBL
WHERE category = 'Clothing'
AND quantity >= 4 
AND TO_CHAR(SALE_DATE, 'YYYY-MM') = '2022-11';


-- CALCULATING THE TOTAL SALES FOR EACH CATEGORY
SELECT category, 
SUM(total_sale) AS total_sales,
COUNT(*) AS total_orders FROM SALES_TBL
GROUP BY category;


-- FINDING AVERAGE AGE OF CUSTOMERS WHO PURCHASED FROM 'BEAUTY' CATEGORY
SELECT ROUND(AVG(AGE), 2) FROM SALES_TBL
WHERE category = 'Beauty';


-- FINDING TRANSACTIONS WHERE TOTAL SALES IS GREATER THAN 1000
SELECT * FROM SALES_TBL
WHERE total_sale > 1000;


-- FINDING TOTAL NUMBER OF TRANSACTIONS ID MADE BY EACH GENDER IN EACH CATEGORY
SELECT category, gender, COUNT(*) FROM SALES_TBL
GROUP BY category, gender
ORDER BY category;


-- CALCULATE AVERAGE SALE FOR EACH MONTH, FINDING THE BEST SELLING MONTH IN EACH YEAR.
SELECT 
	year, month, avg_sale FROM 
(
SELECT 
EXTRACT (YEAR FROM sale_date) as year,
EXTRACT (MONTH FROM sale_date) as month,
AVG(total_sale) AS avg_sale,
RANK () OVER(PARTITION BY EXTRACT (YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
FROM SALES_TBL 
GROUP BY 1, 2
) AS TBL1
WHERE rank = 1;


-- FINDING TOP 5 CUSTOMERS BASED ON THE HIGHEST TOTAL SALES 
SELECT customer_id, SUM(total_sale) as total_Sales FROM SALES_TBL
GROUP BY 1
ORDER BY total_sales DESC
LIMIT 5;


-- FINDING UNIQUE CUSTOMERS WHO PURCHASED FROM EACH CATEGORY
SELECT category, COUNT(DISTINCT(customer_id)) AS unique_cust FROM SALES_TBL
GROUP BY 1;


-- CREATING SHIFT AND FINDING THE NUMBER OF ORDERS
WITH hourly_Sales
AS
(
SELECT *,
	CASE 
	WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
	WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
	ELSE 'Night'
	END AS Shifts
	FROM SALES_TBL
)
SELECT Shifts, COUNT(*) AS total_orders FROM hourly_Sales
GROUP BY Shifts;
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.


