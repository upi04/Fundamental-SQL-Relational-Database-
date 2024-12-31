###  System-Defined Functions
System-defined functions are pre-built SQL functions for string manipulation, date handling, and aggregate computations.

#### **Common Functions:**
- **String Functions:**
  - `UPPER()`: Converts text to uppercase.
    ```sql
    SELECT UPPER('example') AS UpperCaseText;
    ```
  - `CONCAT()`: Concatenates multiple strings.
    ```sql
    SELECT CONCAT(first_name, ' ', last_name) AS FullName FROM users;
    ```

- **Date Functions:**
  - `GETDATE()`: Returns the current system date and time.
    ```sql
    SELECT GETDATE() AS CurrentDateTime;
    ```
  - `DATEDIFF()`: Calculates the difference between two dates.
    ```sql
    SELECT DATEDIFF(DAY, '2022-01-01', GETDATE()) AS DaysDifference;
    ```

- **Aggregate Functions:**
  - `SUM()`, `AVG()`, `COUNT()`, `MIN()`, `MAX()`.
    ```sql
    SELECT AVG(price_usd) AS AveragePrice FROM products;
    ```

---