### ** Common Table Expressions (CTEs)**
CTEs simplify complex queries by allowing temporary naming of result sets.

#### **Example:** Calculate total payments per customer and rank them by amount.
```sql
WITH total_payments AS (
    SELECT id_customer, SUM(jumlah_pembayaran) AS total_amount
    FROM payments
    GROUP BY id_customer
)
SELECT id_customer, total_amount,
       RANK() OVER (ORDER BY total_amount DESC) AS rank
FROM total_payments;
```
**Output:** This query ranks customers by their total payments.

---