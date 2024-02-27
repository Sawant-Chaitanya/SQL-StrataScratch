```SQL
-- Common Table Expression (CTE) to retrieve previous transaction timestamps for each user
WITH cte AS (
  SELECT 
    user_id,
    created_at,
    -- Use LAG window function to get the timestamp of the previous transaction for each user
    LAG(created_at) OVER (PARTITION BY user_id ORDER BY created_at) AS prev_created_at
  FROM 
    amazon_transactions
) 
-- Main query to filter returning active users
SELECT 
  user_id
FROM 
  cte
WHERE 
  -- Filter out rows where the previous transaction timestamp is not available (i.e., for the first transaction)
  prev_created_at IS NOT NULL
  -- Filter transactions that occurred within 7 days of the previous transaction
  AND DATEDIFF(day, prev_created_at, created_at) BETWEEN 0 AND 7
-- Grouping by user_id in case the same user has multiple qualifying transactions
GROUP BY 
  user_id;
```
