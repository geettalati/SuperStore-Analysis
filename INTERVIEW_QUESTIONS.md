# Superstore Analysis - Technical Interview Preparation Guide

This document contains a comprehensive list of 100 interview questions ranging from basic (Level 0) to advanced (Level 100) regarding your Superstore Data Analysis project. **They are intentionally left unanswered** so you can use them as flashcards or study prompts. 

By figuring out the answers to these, you will deeply understand every single line of code in your project and the business logic behind it.

---

## Level 0: The "Elevator Pitch" & Project Basics (Behavioral)
1. Tell me about your Superstore Data Analysis project. What was the main objective?
2. What specific business problems were you trying to solve with this analysis?
3. Walk me through the architecture of this project. What tools did you use and why?
4. What datasets did you use? What is the relationship between `order.csv`, `People.csv`, and `Returns.csv`?
5. If you had to explain your biggest finding to a non-technical stakeholder, what would it be?
6. Why did you choose MySQL for this project instead of PostgreSQL, SQL Server, or a NoSQL database?
7. What challenges did you face during this project and how did you overcome them?
8. If you had 2 more weeks to work on this project, what features or analysis would you add?

## Level 10: Database Design & Schema (DDL)
9. Explain your database schema. How do the `Store`, `People`, and `Returns` tables relate to each other?
10. In the `Store` table, you used `Row_ID INT AUTO_INCREMENT PRIMARY KEY`. What does `AUTO_INCREMENT` do?
11. Why did you add a `UNIQUE (Order_ID, Product_ID)` constraint to the `Store` table? What data integrity issue does this prevent?
12. In the `Returns` table, you have a `FOREIGN KEY (Order_ID) REFERENCES Store(Order_ID) ON DELETE CASCADE`. Explain what `ON DELETE CASCADE` means and why it's useful here.
13. You used `VARCHAR(100)` for many columns. What is the difference between `VARCHAR` and `CHAR`? 
14. What happens in MySQL if you try to insert a string longer than 100 characters into a `VARCHAR(100)` column?
15. Why did you design a separate table for `Returns` instead of just having an `Is_Returned` boolean column in the `Store` table?
16. Does a primary key automatically create an index? What type of index?

## Level 20: Data Import & Admin (DML)
17. How did you load the massive `order.csv` dataset into MySQL?
18. What is the advantage of using `LOAD DATA INFILE` over writing thousands of `INSERT INTO` statements?
19. In your `LOAD DATA INFILE` query, you used `IGNORE 1 ROWS`. Why is this necessary?
20. What do the clauses `FIELDS TERMINATED BY ','` and `ENCLOSED BY '"'` mean when parsing a CSV?
21. Before loading the file, you ran `SHOW VARIABLES LIKE 'secure_file_priv';`. What does this variable do in MySQL?
22. If your CSV file had a date formatted as DD-MM-YYYY, how would you handle that during the `LOAD DATA INFILE` process?
23. What happens if a CSV file is missing a column that the MySQL table expects?

## Level 30: Data Cleaning & Transformation
24. Why were `Order_Date` and `Ship_Date` initially loaded as strings instead of `DATE` types?
25. Explain how the `STR_TO_DATE()` function works. What does `%Y-%m-%d` signify?
26. You used the `ALTER TABLE ... MODIFY` command to change the column types after cleaning. What would have happened if you tried to alter the table before cleaning the strings?
27. For the `Sales` and `Profit` columns, you used `REPLACE(REPLACE(profit, '$', ''), ',', '')`. Walk me through how these nested `REPLACE` functions execute.
28. You cast the cleaned Sales and Profit strings to `FLOAT`. What is the difference between `FLOAT`, `DOUBLE`, and `DECIMAL`? 
29. Why is `DECIMAL` actually the best practice for financial data instead of `FLOAT`?
30. Before updating the date columns, you ran `SET SQL_SAFE_UPDATES = 0;`. What is Safe Update mode in MySQL and why did it block your query?
31. How would you handle a scenario where `Sales` or `Profit` contained `NULL` values?

## Level 40: Basic Queries & Exploratory Data Analysis (EDA)
32. Write a query to find out exactly how many rows are in the `Store` table.
33. How did you write a query to find if there were any missing (NULL) values in the `Postal_Code` column?
34. What is the difference between `COUNT(*)` and `COUNT(Postal_Code)`?
35. You used `GROUP BY Order_ID HAVING COUNT(*) > 1` to find duplicate orders. Explain the difference between the `WHERE` clause and the `HAVING` clause.
36. Can you use an alias created in the `SELECT` clause within a `WHERE` clause? What about within a `GROUP BY` or `HAVING` clause?
37. How did you find the Total Sales for each Product Category?
38. To find the Monthly Sales Trend, you used `MONTHNAME(Order_Date)`. If you want to group by year AND month (e.g., "2016-Jan"), how would you alter that query?
39. Explain how the `CASE` statement works. How did you use it to categorize orders into High, Medium, and Low value?
40. Is it possible to use a `CASE` statement inside an aggregate function, like `SUM(CASE WHEN...)`? Give an example of when you'd use that.

## Level 50: Advanced SQL (Window Functions)
41. What is a Window Function in SQL, and how is it different from a standard aggregate function with a `GROUP BY`?
42. Walk me through your query for calculating the Running Total Sales: `SUM(Sales) OVER (PARTITION BY Region ORDER BY Order_Date)`. What does `PARTITION BY` do here?
43. What would happen to the running total if you removed the `PARTITION BY Region` clause?
44. What would happen if you removed the `ORDER BY Order_Date` clause inside the `OVER()` window?
45. You used `RANK() OVER (ORDER BY SUM(Profit) DESC)` to rank products. What is the difference between `RANK()`, `DENSE_RANK()`, and `ROW_NUMBER()`? 
46. If two products have the exact same total profit, how will `RANK()` assign their ranks? How will `ROW_NUMBER()` assign them?
47. How would you find the top 3 most profitable products in *each* Region using window functions?
48. What is the `NTILE()` window function and how could it be used to find the top 25% (quartile) of customers by sales?

## Level 60: Complex Logic (CTEs and Lags)
49. What is a Common Table Expression (CTE)? What is the syntax for creating one using the `WITH` keyword?
50. Why did you use a CTE to calculate the Month-over-Month Sales Growth instead of writing it all in one query block?
51. Can a CTE reference another CTE?
52. Explain the `LAG()` function. How did you use it to get the `Prev_Month_Sales`?
53. Does `LAG()` require an `ORDER BY` clause inside its `OVER()` window? Why?
54. What is the opposite of the `LAG()` function, and when might you use it?
55. How did you handle the fact that the very first month has no previous month to compare to? What does the `LAG()` function return in that case?
56. How is calculating Year-over-Year growth different from Month-over-Month growth when using `LAG()`?

## Level 70: Market Basket Analysis (Self Joins)
57. What is a Self Join?
58. In your Market Basket query, you joined the `Store` table to itself on `o1.Order_ID = o2.Order_ID`. What does this achieve?
59. Crucially, you added `AND o1.Product_Name < o2.Product_Name`. Why use the less than `<` operator? 
60. What would happen if you used `<>` (not equal to) instead of `<` in the self join condition?
61. If an order had 3 distinct items in it (A, B, and C), how many pairs would your Self Join generate for that single order?
62. How would you modify your Market Basket query if you wanted to find combinations of 3 products instead of 2?
63. What is the business value of discovering that Product A and Product B are frequently bought together? How would a company use this information?

## Level 80: Multi-Table Joins & Operations
64. Explain the core differences between an `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, and `FULL OUTER JOIN`.
65. Does MySQL support `FULL OUTER JOIN` natively? If not, how do you simulate it?
66. To calculate the overall Return Rate, you used a `LEFT JOIN` from the `Store` table to the `Returns` table. Why not an `INNER JOIN`? 
67. What would an `INNER JOIN` have calculated in that scenario instead?
68. Write a query out loud to find the names of regional managers (from the `People` table) who have the highest number of returned orders in their region.
69. How did you calculate the average Delivery Delay? Explain how the `DATEDIFF()` function works.
70. Does `DATEDIFF(date1, date2)` calculate `date1 - date2` or `date2 - date1`?
71. What is a Subquery? 
72. Why might you use a subquery inside a `WHERE` clause (e.g., `WHERE Order_ID IN (SELECT...)`) instead of joining two tables? 
73. What is a Correlated Subquery and how is it different from a standard subquery?

## Level 90: Query Optimization & Performance 
74. The `order.csv` file has thousands of rows, but imagine it had 500 million rows. Your `GROUP BY Category` query is suddenly taking 10 minutes to run. How do you investigate the bottleneck?
75. What is an Index in a database? How does it work under the hood (e.g., B-Trees)?
76. If you frequently run queries filtering by `Order_Date` and `Region`, how would you create a composite index for this?
77. Does the order of columns in a composite index matter? (e.g., `INDEX(Region, Order_Date)` vs `INDEX(Order_Date, Region)`).
78. What is the downside of adding too many indexes to a table?
79. Explain how the `EXPLAIN` keyword works in MySQL. What do "type: ALL" or "Using filesort" mean in the `EXPLAIN` output?
80. Your running total Window Function requires sorting the data (`ORDER BY Order_Date`). Sorting is computationally expensive. How can indexing help here?
81. What is database normalization? What normal form is your `Store` table currently in?
82. If you were to fully normalize the `Store` table, what new tables would you create? (e.g., Customers, Products, Addresses).
83. What is the trade-off between a highly normalized database vs a denormalized database for analytics (OLTP vs OLAP)?
84. What is a View in SQL? 
85. If the marketing team needs to see the "High Value Customers" query every day, would you create a View or just save the SQL script?
86. What is a Materialized View? Does MySQL support them natively?

## Level 100: Business Intelligence, Logic Interpretation & Strategy
87. You calculated the "Profit Lost due to Returns". Does a return strictly mean a total loss of the sales price, or just the loss of the profit margin? 
88. How should a business actually account for returns in their financial reporting based on your data?
89. You found out which `Ship_Mode` has the highest return rate. What hypothesis would you form based on this? 
90. If "Same Day" shipping has a drastically higher return rate than "Standard Class", what actionable advice would you give the logistics team?
91. Looking at discounts, how would you write a query to test the hypothesis: "Giving higher discounts leads to higher sales volume, but actually destroys net profit"?
92. If a specific product sub-category (e.g., Tables) is generating massive sales but negative profit, what are three recommendations you would give to the management team?
93. How would you identify "Churned" customers? (e.g., customers who haven't made a purchase in the last 6 months).
94. Explain the concept of RFM Analysis (Recency, Frequency, Monetary). How would you write SQL to segment customers into RFM tiers?
95. If you wanted to predict which orders are most likely to be returned *before* they ship, what data points from this dataset would you feed into a machine learning model?
96. How would you calculate Customer Lifetime Value (CLV) using the `Store` table?
97. How would you structure this data if you wanted to connect it to a BI dashboard (like Tableau or PowerBI) for a live daily feed?
98. What is a Star Schema? How would you convert this dataset into a Star Schema with Fact and Dimension tables?
99. If this data pipeline needed to run automatically every night at 2 AM, how would you orchestrate that in a real-world enterprise environment?
100. Overall, what is the single most important metric for the Superstore's survival, and how does your SQL analysis track it?
