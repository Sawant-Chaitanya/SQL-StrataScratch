```SQL
-- Selecting the sender (from_user) and the count of emails sent by each sender
SELECT 
    from_user, 
    COUNT(*) AS total_emails, 
    -- Ranking the senders based on the count of emails they sent (in descending order),
    -- and then by from_user in ascending order (alphabetical order)
    ROW_NUMBER() OVER (ORDER BY COUNT(*) DESC, from_user) AS row_number
FROM 
    google_gmail_emails
GROUP BY 
    from_user; 
```
