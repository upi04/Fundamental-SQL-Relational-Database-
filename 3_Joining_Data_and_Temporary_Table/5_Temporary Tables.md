### ** Temporary Tables**
Temporary tables store intermediate results during query execution. They exist only for the duration of the session.

#### **Example:** Create a temporary table for high-value payments.
```sql
CREATE TEMPORARY TABLE high_value_payments AS
SELECT *
FROM payments
WHERE jumlah_pembayaran > 300000;

SELECT * FROM high_value_payments;
```
**Output:** This query creates a temporary table with payments above 300,000 and retrieves its contents.

---