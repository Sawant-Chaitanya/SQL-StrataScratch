```SQL
-- Main query to calculate the salary difference between the Marketing and Engineering departments
SELECT 
    (MAX(CASE WHEN department = 'marketing' THEN salary END) - -- Maximum salary in the Marketing department
     MAX(CASE WHEN department = 'engineering' THEN salary END)) -- Maximum salary in the Engineering department
     AS salary_difference 
FROM 
    db_employee 
INNER JOIN 
    db_dept ON db_employee.department_id = db_dept.id;  

````
