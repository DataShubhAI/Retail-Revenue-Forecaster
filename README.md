# 🛒 Retail Revenue Forecaster - P1

This project is a comprehensive **SQL-based data analysis** and **forecasting solution** for retail sales data. It covers everything from **data creation and cleaning** to **exploratory data analysis (EDA)** and **answering critical business questions** using SQL queries.

---

## 📂 Project Structure

- **Database Creation**
- **Table Creation**
- **Data Cleaning (handling NULLs)**
- **Exploratory Data Analysis (EDA)**
- **Business Queries and Key Insights**

---

## 🛠️ Technologies Used

- SQL PostgreSQL 
- DBMS:pgAdmin 4
- Dataset: `retail_sales` (custom created)

---

## 📌 Business Questions Answered

1. All sales made on a specific date (`2022-11-05`)
2. Clothing category sales with quantity > 4 in November 2022
3. Category-wise total sales
4. Average age of customers in the Beauty category
5. Transactions with total sales> ₹1000
6. Gender and category-wise transaction counts
7. Average sales by month and best month per year
8. Top 5 customers based on the highest total sales
9. Unique customer count per category
10. Time-shift-wise order distribution

---
Business Analysis & SQL Queries
This section addresses key business questions with their corresponding SQL queries and solutions.

**Q1: Retrieve all sales made on '2022-11-05'.**

SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';

Q2: Find transactions for 'Clothing' where quantity is 4 or more during November 2022.

SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND quantiy >= 4
  AND sale_date >= '2022-11-01'
  AND sale_date < '2022-12-01';

**Q3: Calculate the total sales for each category.**

SELECT category, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY category;

**Q4: Find the average age of customers who purchased 'Beauty' products.**

SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';

**Q5: Find all transactions where the total sale was greater than 1000.**

SELECT transactions_id, total_sale
FROM retail_sales
WHERE total_sale > 1000;

**Q6: Count the number of transactions made by each gender in each category.**

SELECT category, gender, COUNT(transactions_id) AS total_trans
FROM retail_sales
GROUP BY category, gender;

**Q7: Find the best-selling month (by average sale) for each year.**

SELECT year, month, avg_sale
FROM (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS ranked_sales
WHERE rank = 1;

**Q8: Find the top 5 customers based on their total spending.**

SELECT customer_id, SUM(total_sale) AS highest_sale
FROM retail_sales
GROUP BY customer_id
ORDER BY highest_sale DESC
LIMIT 5;

**Q9: Find the number of unique customers for each category.**

SELECT category, COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;

**Q10: Group sales into shifts (Morning, Afternoon, Evening) and count transactions in each.**

WITH hourly_sale AS (
    SELECT
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT shift, COUNT(*) AS total_sales
FROM hourly_sale
GROUP BY shift;


## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `RRF Qry.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `RRF Qry.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Shubham Yadav

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

### Stay Updated and Join the Community

For more content on SQL, data analysis, and other data-related topics, make sure to follow me on social media and join our community:

- **LinkedIn**: https://www.linkedin.com/in/shubham-yadav-98a0a4286/

Thank you,  and I look forward to connecting with you!




