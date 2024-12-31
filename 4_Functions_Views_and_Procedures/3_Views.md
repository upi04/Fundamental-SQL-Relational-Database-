###  Views
Views are virtual tables created by a SELECT query. They simplify complex queries and abstract database logic.

#### **Example:** Create a view for high-value orders.
```sql
CREATE VIEW HighValueOrders AS
SELECT order_id, user_id, price_usd
FROM orders
WHERE price_usd > 500;

SELECT * FROM HighValueOrders;
```

**Updating a View:**
```sql
CREATE OR REPLACE VIEW HighValueOrders AS
SELECT order_id, user_id, price_usd, created_at
FROM orders
WHERE price_usd > 500;
```

---
