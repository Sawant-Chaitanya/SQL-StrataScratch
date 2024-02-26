```SQL
-- Common Table Expression (CTE) to calculate session time differences for each user
WITH session AS
(
    SELECT 
        user_id,
        -- Calculate session time difference in seconds and round it to 2 decimal places
        CAST(
            ROUND(
                DATEDIFF(second,
                    MAX(CASE WHEN action = 'page_load' THEN timestamp END), -- Get latest page_load timestamp
                    MIN(CASE WHEN action = 'page_exit' THEN timestamp END) -- Get earliest page_exit timestamp
                ),
                2
            ) AS DECIMAL(18, 2) -- Cast to DECIMAL with appropriate precision and scale
        ) AS session_time_diff
    FROM 
        facebook_web_log
    GROUP BY 
        user_id,
        DAY(timestamp) -- Group by user_id and day of the timestamp
)

-- Main query to calculate the average session time for each user
SELECT
    user_id,
    AVG(session_time_diff) AS session_time -- Calculate average session time
FROM 
    session
WHERE 
    session_time_diff IS NOT NULL -- Filter out null session time differences
GROUP BY 
    user_id; -- Group by user_id
```
