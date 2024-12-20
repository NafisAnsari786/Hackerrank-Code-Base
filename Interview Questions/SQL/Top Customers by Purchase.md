<img src="https://img.shields.io/badge/MEDIUM-orange" alt="MEDIUM" width="70">

![image](https://github.com/user-attachments/assets/71c23611-e6a8-4f46-8728-091c1ec030e9)

**SOLUTION**
```SQL
WITH TopCust AS ( 
    SELECT 
        CUSTOMERID, 
        REGION, 
        PURCHASEAMOUNT,
        ROW_NUMBER() OVER (PARTITION BY REGION ORDER BY PURCHASEAMOUNT DESC) AS rownum
    FROM CUSTOMERPURCHASE
)
SELECT 
    REGION, 
    CUSTOMERID, 
    SUM(PURCHASEAMOUNT) AS TOTAL_PURCHASEAMOUNT
FROM TopCust
WHERE rownum <= 3
GROUP BY REGION, CUSTOMERID;
```

![image](https://github.com/user-attachments/assets/caaa9817-a04b-4066-8fd9-ae8e0531aa95)

```txt
NOTE

Even if CUSTOMERID is unique within a specific context, if there are multiple purchase records for the same customer (for example, a customer can make multiple purchases over time), then those rows are not automatically aggregated by default.
By grouping by CUSTOMERID, you tell SQL to aggregate (in this case, sum) all the purchase amounts for each customer, rather than treating each purchase record as a separate entity.
```
