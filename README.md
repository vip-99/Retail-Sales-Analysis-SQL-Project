# Retail-Sales-Analysis-SQL-Project


## Project Overview

**Project Title**: Retail Sales Analysis  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. 
The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. 
This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

 **Database Creation**: The project starts by creating a database named `retail_sales`.
 **Table Creation**: A table named `retail_sales` is created to store the sales data. 
The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE retail_sales;

CREATE TABLE retail_sales
(
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
```

### 2. Data Exploration & Cleaning

**Record Count**: Determine the total number of records in the dataset.
**Customer Count**: Find out how many unique customers are in the dataset.
**Category Count**: Identify all unique product categories in the dataset.
**Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE
      transactions_id IS NULL OR sale_date IS NULL OR
      sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR
      quantity IS NULL OR price_per_unit IS NULL OR
      cogs IS NULL OR toal IS NULL;

DELETE FROM retail_sales
WHERE 
      transactions_id IS NULL OR sale_date IS NULL OR
      sale_time IS NULL OR customer_id IS NULL OR 
      gender IS NULL OR age IS NULL OR category IS NULL OR
      quantity IS NULL OR price_per_unit IS NULL OR
      cogs IS NULL OR toal IS NULL;

```

### 3. Data Analysis & Findings


SELECT * FROM retail_sales


1 **How many total sales we have**?
SELECT COUNT(*) AS total_sales From retail_sales;



2 **How many unique Customers we have**?
SELECT COUNT(DISTINCT customer_id) AS total_customers From retail_sales;


3 **How many unique Category we have**?
SELECT category FROM retail_sales;




---------------------------BUSINESS PROBLEMS---------------------------------------

The following SQL queries were developed to answer specific business questions:


1 **Write a SQL query to retrieve all columns for sales made on '2022-11-05'**?

SELECT * FROM retail_sales WHERE sale_date='2022-11-05';


	
2  **Write a SQL query to retrieve all transactions where the category is 'Clothing' and 
the quantity sold is more than 4 in the month of Nov-2022**?

SELECT * FROM retail_sales 
WHERE category='Clothing' 
	AND quantity>=4 
	AND to_char(sale_date,'mm-yyyy')='11-2022';



3 **Write a SQL query to calculate the total sales (total_sale) for each category**?

SELECT category,SUM(total_sale) AS total_sales 
FROM retail_sales 
GROUP BY category
ORDER BY total_sales DESC;



4 **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category**?

SELECT ROUND(AVG(age)::NUMERIC ,1) AS average_age 
FROM retail_sales 
WHERE category='Beauty'
ORDER BY average_age DESC;



5  **Write a SQL query to find all transactions where the total_sale is greater than 1000**?

SELECT * FROM retail_sales WHERE total_sale >1000;



6 **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category**?

SELECT category,gender,COUNT(transactions_id) AS num_of_transactions 
FROM retail_sales 
GROUP BY category,gender
ORDER BY category DESC;



7 **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**?

WITH best_selling_year AS (
	
SELECT to_char(sale_date,'MONTH') AS month ,to_char(sale_date,'YYYY') AS year,
ROUND(AVG(total_sale)::NUMERIC ,1) AS avg_sale,
RANK()OVER(PARTITION BY to_char(sale_date,'YYYY')  ORDER BY AVG(total_sale) DESC) AS rn
FROM retail_sales
GROUP BY 1,2
	)
SELECT year,month,avg_sale FROM best_selling_year WHERE rn=1;




8 **Write a SQL query to find the top 5 customers based on the highest total sales**?

SELECT customer_id AS customer,SUM(total_sale) AS total_sales 
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;


9 **Write a SQL query to find the number of unique customers who purchased items from each category**?
SELECT category,COUNT(DISTINCT customer_id) AS count_of_unique_customers 
FROM retail_sales
GROUP BY category
ORDER BY num_of_unique_customers DESC;


10 **Write a SQL query to create each shift and number of orders**?
(Example Morning <12, Afternoon Between 12 & 17, Evening >17)


WITH shifts AS (	
	
SELECT * ,
  CASE
     WHEN EXTRACT(HOUR FROM sale_time)<12 THEN 'morning'
     WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'afternoon'
     ELSE 'evening'
END AS shift 
FROM retail_sales
	)
SELECT COUNT(*),shift FROM shifts GROUP BY shift;



11 **which Category has highest Cost of goods sold (COGS)**?

SELECT category,ROUND(SUM(cogs):: NUMERIC,1) AS total_cogs 
FROM retail_sales
GROUP BY category
ORDER BY total_cogs DESC;


--------------------**END**----------------------------
