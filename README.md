# SQL Project - Apple Retail Sales Data (Analyzing millions of rows)

# ![Apple Logo](https://github.com/najirh/Apple-Retail-Sales-SQL-Project---Analyzing-Millions-of-Sales-Rows/blob/main/Apple_Changsha_RetailTeamMembers_09012021_big.jpg.slideshow-xlarge_2x.jpg)

## Project Overview

This project is designed to showcase advanced SQL querying techniques through the analysis of over 1 million rows of Apple retail sales data. The dataset includes information about products, stores, sales transactions, and warranty claims across various Apple retail locations globally. By tackling a variety of questions, from basic to complex, you'll demonstrate your ability to write sophisticated SQL queries that extract valuable insights from large datasets.

## Entity Relationship Diagram (ERD)

![ERD](https://github.com/najirh/Apple-Retail-Sales-SQL-Project---Analyzing-Millions-of-Sales-Rows/blob/main/erd.png)

## Database Schema

The project uses five main tables:

1. **stores**: Contains information about Apple retail stores.
   - `store_id`: Unique identifier for each store.
   - `store_name`: Name of the store.
   - `city`: City where the store is located.
   - `country`: Country of the store.

2. **category**: Holds product category information.
   - `category_id`: Unique identifier for each product category.
   - `category_name`: Name of the category.

3. **products**: Details about Apple products.
   - `product_id`: Unique identifier for each product.
   - `product_name`: Name of the product.
   - `category_id`: References the category table.
   - `launch_date`: Date when the product was launched.
   - `price`: Price of the product.

4. **sales**: Stores sales transactions.
   - `sale_id`: Unique identifier for each sale.
   - `sale_date`: Date of the sale.
   - `store_id`: References the store table.
   - `product_id`: References the product table.
   - `quantity`: Number of units sold.

5. **warranty**: Contains information about warranty claims.
   - `claim_id`: Unique identifier for each warranty claim.
   - `claim_date`: Date the claim was made.
   - `sale_id`: References the sales table.
   - `repair_status`: Status of the warranty claim (e.g., Paid Repaired, Warranty Void).

## Business Questions

### Easy to Medium (10 Questions)

1. Find the number of stores in each country.
```
SELECT 
   country,
   count(store_id) as total_stores
   from stores
GROUP BY country
ORDER BY total_stores DESC;
```
![image](https://github.com/user-attachments/assets/7cf0e4b0-4817-4a82-99d5-89368a0ad2e1)

2. Calculate the total number of units sold by each store.
   
4. Identify how many sales occurred in December 2023.
5. Determine how many stores have never had a warranty claim filed.
6. Calculate the percentage of warranty claims marked as "Warranty Void".
7. Identify which store had the highest total units sold in the last year.
8. Count the number of unique products sold in the last year.
9. Find the average price of products in each category.
10. How many warranty claims were filed in 2020?
11. For each store, identify the best-selling day based on highest quantity sold.

### Medium to Hard (5 Questions)

11. Identify the least selling product in each country for each year based on total units sold.
12. Calculate how many warranty claims were filed within 180 days of a product sale.
13. Determine how many warranty claims were filed for products launched in the last two years.
14. List the months in the last three years where sales exceeded 5,000 units in the USA.
15. Identify the product category with the most warranty claims filed in the last two years.

### Complex (5 Questions)

16. Determine the percentage chance of receiving warranty claims after each purchase for each country.
17. Analyze the year-by-year growth ratio for each store.
18. Calculate the correlation between product price and warranty claims for products sold in the last five years, segmented by price range.
19. Identify the store with the highest percentage of "Paid Repaired" claims relative to total claims filed.
20. Write a query to calculate the monthly running total of sales for each store over the past four years and compare trends during this period.
21. Analyze product sales trends over time, segmented into key periods: from launch to 6 months, 6-12 months, 12-18 months, and beyond 18 months.

## Dataset

- **Size**: 1 million+ rows of sales data.
- **Period Covered**: The data spans multiple years, allowing for long-term trend analysis.
- **Geographical Coverage**: Sales data from Apple stores across various countries.

## Conclusion
The Apple Retail Sales Data Analysis project showcases the power and versatility of SQL in deriving actionable insights from vast and complex datasets. By analyzing over 1 million rows of data, we have:

Enhanced Understanding: Explored sales patterns, warranty claims, and product performance across global Apple stores.
- **Key Insights:** Identified top-performing stores, best-selling days, and product categories with the highest warranty claims.
- **Advanced Techniques:** Applied ranking, window functions, and subqueries to answer challenging business questions, demonstrating proficiency in handling real-world SQL scenarios.
- **Strategic Recommendations:** Provided insights into customer preferences, product lifecycle performance, and warranty claim trends, aiding strategic decision-making.
This project not only highlights the analytical potential of SQL but also underscores its critical role in supporting data-driven business strategies in the retail domain.
