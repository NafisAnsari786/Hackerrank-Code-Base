<img src="https://img.shields.io/badge/MEDIUM-orange" alt="MEDIUM" width="70">

![image](https://github.com/user-attachments/assets/d807d469-45c7-4d4d-92f9-17f093dd1f0f)

**SOLUTION**
```sql
WITH GetMonth AS (
    SELECT 
        customer_id, 
        SUM(total_amount) AS total_spent
    FROM Orders
    WHERE order_date >= DATE_TRUNC('month', CURRENT_DATE) - INTERVAL '1 month'
      AND order_date < DATE_TRUNC('month', CURRENT_DATE)
    GROUP BY customer_id
),
TopCust AS (
    SELECT 
        customer_id, 
        total_spent,
        DENSE_RANK() OVER (ORDER BY total_spent DESC) AS rank
    FROM GetMonth
)
SELECT customer_id, total_spent
FROM TopCust
WHERE rank <= 5
ORDER BY total_spent DESC;
```
