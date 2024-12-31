### ** Materialized Views**
Materialized views store query results physically, improving performance for expensive computations.

#### **Creating a Materialized View:**
```sql
CREATE MATERIALIZED VIEW SalesSummary AS
SELECT product_id, SUM(price_usd) AS TotalSales
FROM order_items
GROUP BY product_id;

SELECT * FROM SalesSummary;
```

**Refreshing a Materialized View:**
- Manual refresh:
  ```sql
  REFRESH MATERIALIZED VIEW SalesSummary;
  ```
- Automatic refresh can be configured using database-specific options.

---
