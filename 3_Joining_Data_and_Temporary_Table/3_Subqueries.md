### ** Subqueries**
A subquery is a query nested inside another query.

#### **Example:** Retrieve customers with payments above the average amount.
```sql
SELECT nama_customer, jumlah_pembayaran
FROM customers
JOIN payments USING(id_customer)
WHERE jumlah_pembayaran > (
    SELECT AVG(jumlah_pembayaran) FROM payments
);
```
**Output:** This query retrieves customers whose payment amounts exceed the average payment.

---