### ** Functions**
Functions are reusable SQL code blocks that can perform operations and return results. Functions can be scalar (return a single value) or table-valued (return a table).

#### **Example:**
**Scalar Function:** Calculate the total price including tax for a product.
```sql
CREATE FUNCTION CalculateTotalPrice(@price DECIMAL(6,2), @tax_rate DECIMAL(4,2))
RETURNS DECIMAL(6,2)
AS
BEGIN
    RETURN @price + (@price * @tax_rate);
END;

SELECT dbo.CalculateTotalPrice(100.00, 0.07) AS TotalPrice;
```

**Table-Valued Function:** Return all orders for a specific user.
```sql
CREATE FUNCTION GetOrdersByUser(@user_id BIGINT)
RETURNS TABLE
AS
RETURN (
    SELECT * FROM orders WHERE user_id = @user_id
);

SELECT * FROM GetOrdersByUser(1);
```

---
