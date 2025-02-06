# MySQL Case Study- Apple Retail Sales Data Analysis

## Business Questions

### 1. Find the number of stores in each country.
```
SELECT 
   country,
   count(store_id) as total_stores
   from stores
GROUP BY country
ORDER BY total_stores DESC;
```
### 2. What is the total number of units sold by each store?
```
SELECT stores.store_name, SUM(sales.quantity) AS Total_Units_Sold
FROM sales
INNER JOIN stores ON sales.store_id = stores.store_id
GROUP BY stores.store_name
ORDER BY Total_Units_Sold DESC;
```
### 3. How many sales occurred in December 2023?
```
SELECT COUNT(*) AS total_sales
FROM sales
WHERE YEAR(sale_date) = 2023 AND MONTH(sale_date) = 12;
```
### 4. How many stores have never had a warranty claim filed against any of their products?
```
SELECT COUNT(*) FROM stores
WHERE store_id NOT IN (
SELECT 
DISTINCT store_id
FROM sales as s
RIGHT JOIN warranty as w
ON s.sale_id = w.sale_id
);
```
### 5. What percentage of warranty claims are marked as "Warranty Void"?
```
SELECT 
    (warranty_void_claims / total_claims) * 100 AS warranty_void_percentage
FROM 
    (SELECT COUNT(*) as Total_claims FROM warranty) as t,
	(SELECT COUNT(*) as "Warranty_Void_Claims" FROM warranty WHERE repair_status = "Warranty Void")as w;
```

### 6. Which store had the highest total units sold in the last year?
```
SELECT st.store_name, SUM(s.quantity) AS Total_Units_Sold
FROM sales as s
INNER JOIN stores as st ON s.store_id = st.store_id
WHERE sale_date >= (CURRENT_DATE - INTERVAL 1 year)
GROUP BY st.store_name
ORDER BY Total_Units_Sold DESC
LIMIT 1;
```
### 7. Count the number of unique products sold in the last year
```
SELECT COUNT(DISTINCT product_name) AS Total_Products
from products as p
INNER JOIN sales as s ON s.product_id = p.product_id
WHERE sale_date >= (CURRENT_DATE - INTERVAL 1 year);
```
### 8. What is the average price of products in each category?
```
SELECT c.category_id, c.category_name, avg(p.price) as average_price_of_products
from category as c
INNER JOIN products as p ON c.category_id = p.category_id
GROUP BY category_id, category_name
ORDER BY average_price_of_products DESC;
```
### 9. How many warranty claims were filed in 2020?
```
SELECT count(*) AS warranty_claim from warranty 
WHERE YEAR(claim_date) = 2020;
```
### 10. Identify each store and best selling day based on highest qty sold
```
WITH RankedSales AS (
    SELECT 
        st.store_id,
        st.store_name, 
        DAYNAME(s.sale_date) AS day_name,
        SUM(s.quantity) AS total_quantity_sold,
        RANK() OVER (PARTITION BY st.store_id ORDER BY SUM(s.quantity) DESC) AS rank_val
    FROM stores AS st
    INNER JOIN sales AS s ON st.store_id = s.store_id
    GROUP BY st.store_id, st.store_name, DAYNAME(s.sale_date)
)
SELECT 
    store_id,
    store_name,
    day_name AS best_selling_day,
    total_quantity_sold
FROM RankedSales
WHERE rank_val = 1;
```
### 11.How many warranty claims were filed within 180 days of a product sale?
```
SELECT 
    COUNT(*)
FROM warranty AS w
INNER JOIN sales AS s
ON s.sale_id = w.sale_id
WHERE DATEDIFF(w.claim_date, s.sale_date) <= 180;
```
### 12. How many warranty claims have been filed for products launched in the last two years?
```
SELECT 
    p.product_name,
    COUNT(w.claim_id) AS no_claim,
    COUNT(s.sale_id) AS no_sales
FROM warranty AS w
RIGHT JOIN sales AS s 
ON s.sale_id = w.sale_id
JOIN products AS p
ON p.product_id = s.product_id
WHERE p.launch_date >= (CURRENT_DATE - INTERVAL 2 YEAR)
GROUP BY p.product_name
HAVING COUNT(w.claim_id) > 0;
```

### 13.  List the months in the last 3 years where sales exceeded 5000 units from USA.
```
SELECT 
    DATE_FORMAT(s.sale_date, '%m-%Y') AS month,
    SUM(s.quantity) AS total_unit_sold
FROM sales AS s
JOIN stores AS st
    ON s.store_id = st.store_id
WHERE 
    st.country = 'UK'
    AND s.sale_date >= CURRENT_DATE - INTERVAL 3 YEAR
GROUP BY month
HAVING SUM(s.quantity) > 5000;
```

### 14. Which product category had the most warranty claims filed in the last 2 years?
```
SELECT 
	c.category_name,
	COUNT(w.claim_id) as total_claims
FROM warranty as w
LEFT JOIN
sales as s
ON w.sale_id = s.sale_id
JOIN products as p
ON p.product_id = s.product_id
JOIN 
category as c
ON c.category_id = p.category_id
WHERE 
	w.claim_date >= CURRENT_DATE - INTERVAL '2 year'
GROUP BY 1
```
