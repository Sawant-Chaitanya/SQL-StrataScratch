```SQL
-- Common Table Expression (CTE) to filter and categorize product launches by year and company
WITH product_launches AS (
    SELECT 
        company_name,
        CASE WHEN year = 2020 THEN product_name END AS prod_2020, -- Products launched in 2020
        CASE WHEN year = 2019 THEN product_name END AS prod_2019  -- Products launched in 2019
    FROM 
        car_launches
)
-- Main query to calculate the net number of products launched by each company
SELECT 
    company_name, 
    COUNT(prod_2020) - COUNT(prod_2019) AS net_products 
FROM 
    product_launches
GROUP BY 
    company_name; 
```
