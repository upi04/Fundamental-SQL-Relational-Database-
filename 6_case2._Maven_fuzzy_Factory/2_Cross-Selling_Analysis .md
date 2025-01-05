# Cross-Selling Analysis Using SQL

Below is an example of how you might analyze cross-selling effectiveness using SQL. We assume you have a sales-related table (e.g., `sales_transactions`) containing columns such as:

- `id_customer`  
- `id_transaksi`  
- `tgl_transaksi`  
- `jumlah_pembayaran`  

In the case described, **cross-selling** was implemented starting in December. The company wants to compare **sales before and after** cross-sell, which effectively means comparing November vs. December sales (and potentially beyond) to see if there was an increase.

---

## 1. Monthly Sales Trend

### Query

```sql
SELECT
    DATE_TRUNC('month', tgl_transaksi) AS month_start,
    SUM(jumlah_pembayaran) AS total_sales
FROM sales_transactions
GROUP BY DATE_TRUNC('month', tgl_transaksi)
ORDER BY month_start;
```
Explanation
DATE_TRUNC('month', tgl_transaksi): Rounds each transaction date to the first day of its month (e.g., 2021-11-01 00:00:00 for November).
SUM(jumlah_pembayaran): Totals the sales for all transactions within each month.
GROUP BY: Groups the data by monthly periods.
ORDER BY month_start: Sorts results in ascending order by month.
Result: A table showing each month’s starting date and the total sales (revenue) for that month. This gives you an overview of how monthly sales are trending, which helps identify any increases or decreases over time.
## 2. Comparing Month-over-Month Sales (Optional Window Function)
If your SQL dialect supports window functions (e.g., PostgreSQL, SQL Server, Oracle), you can calculate the sales difference from one month to the next:

```sql
WITH monthly_sales AS (
    SELECT
        DATE_TRUNC('month', tgl_transaksi) AS month_start,
        SUM(jumlah_pembayaran) AS total_sales
    FROM sales_transactions
    GROUP BY DATE_TRUNC('month', tgl_transaksi)
)
SELECT
    month_start,
    total_sales,
    LAG(total_sales) OVER (ORDER BY month_start) AS prev_month_sales,
    (total_sales - LAG(total_sales) OVER (ORDER BY month_start)) AS diff_sales
FROM monthly_sales
ORDER BY month_start;

```
Explanation
LAG(total_sales): Accesses the total_sales value from the previous month in the sorted list.
diff_sales: Shows how much sales changed from the previous month (positive = increase, negative = decrease).
By comparing the November row to the December row, you can directly see if the cross-selling strategy aligned with an increase (or decrease) in monthly revenue.
3. Focused Comparison: November vs. December
Since the cross-sell strategy began in December, you may want a simpler before/after snapshot:
```sql
-- November Sales
SELECT
    SUM(jumlah_pembayaran) AS total_sales_nov
FROM sales_transactions
WHERE tgl_transaksi >= '2021-11-01'
  AND tgl_transaksi <  '2021-12-01';

-- December Sales
SELECT
    SUM(jumlah_pembayaran) AS total_sales_dec
FROM sales_transactions
WHERE tgl_transaksi >= '2021-12-01'
  AND tgl_transaksi <  '2022-01-01';
```
Explanation
First query sums up all sales from November 1st up to (but not including) December 1st.
Second query sums up all sales from December 1st up to (but not including) January 1st (assuming the cross-selling effect is measured within December).
You can then compare total_sales_nov vs. total_sales_dec:

If December > November, it suggests a positive impact from cross-selling.
If December <= November, you might explore other factors (seasonality, marketing changes, etc.).