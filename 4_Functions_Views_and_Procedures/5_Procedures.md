### ** Procedures**
Stored procedures encapsulate SQL statements for reuse, enabling parameterized execution and better performance.

#### **Creating a Procedure:**
```sql
CREATE PROCEDURE UpdateProductPrice
    @product_id INT,
    @new_price DECIMAL(6,2)
AS
BEGIN
    UPDATE products
    SET price_usd = @new_price
    WHERE product_id = @product_id;
END;
```